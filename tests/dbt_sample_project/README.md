# DBT Sample Project

This is a sample dbt project used for testing purposes. It provides a minimal but functional dbt setup with example models and configurations.

## Project Structure

```
dbt_sample_project/
├── models/              # Contains dbt models
│   ├── customers.sql    # Customer data model
│   ├── orders.sql      # Orders data model
│   └── some_other_model.sql
├── macros/             # Custom dbt macros
├── seeds/              # Seed data files
├── dbt_project.yml     # Project configuration
└── profiles.yml        # Connection profiles
```

## Configuration

- Project name: `dbt_changed_models`
- Default materialization: view
- Database: DuckDB (configured in profiles.yml)

## Models

The project includes three simple models:
- `customers.sql`: Customer data model
- `orders.sql`: Orders data model
- `some_other_model.sql`: Additional model for testing

## Getting Started

1. Ensure you have dbt installed
2. The project uses DuckDB as the database backend
3. Run `dbt deps` to install dependencies
4. Run `dbt build` to execute the models

## Testing

This project is primarily used for testing the dbt-changed-models functionality. It provides a controlled environment for testing model changes and their dependencies. 