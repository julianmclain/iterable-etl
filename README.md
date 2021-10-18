# Iterable Data Import

A library to simplify bulk importing data into Iterable. You write the transformations, and the library handles the extract and load process.

## Getting Started

install from pypi

## Usage

At a high level, the library is used by constructing an `IterableDataImport` instance and calling `IterableDataImport.run` to initiate the import.

```python3
import pathlib

from iterable_data_import import (
    IterableDataImport,
    FileFormat,
    UserProfile,
    ImportAction,
    UpdateUserProfile,
    SourceDataRecord,
)

if __name__ == "__main__":
    source_path = pathlib.Path(__file__).parent / "data.json"
    source_format = FileFormat.NEWLINE_DELIMITED_JSON
    api_key = "some_api_key"

    def map_function(record: SourceDataRecord) -> ImportAction:
        email = record["email"]
        user_id = record["id"]
        data_fields = {"ltv": record["lifetime_value"]}
        user = UserProfile(email, user_id, data_fields)
        return UpdateUserProfile(user)

    idi = IterableDataImport.create(
        api_key, source_path, source_format, map_function, dry_run=True
    )
    idi.run()
```

## Advanced Usage

wiring up an instance without the create factory