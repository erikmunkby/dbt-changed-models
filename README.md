# dbt-changed-models

🔍 **GitHub Action to detect changed dbt models in a Pull Request**  
This action compares the current PR branch against its base and outputs:
- A list of modified or new dbt models
- A ready-to-use `--select` statement for downstream dbt runs

Useful for running targeted CI checks or partial dbt builds only when models change.


## 📦 Features
- Detects models with `state:modified` or `state:new`
- Outputs newline-separated model list
- Outputs prepared dbt `--select` statement including all changes
- Easy to integrate in any dbt CI pipeline

Additionally, with `state:modified` any models dependant on a macro will also be listed.
For more information see [dbt select by state](https://docs.getdbt.com/reference/node-selection/methods#state)

For more info:
- Check out [demo project](./tests/dbt_sample_project/)
- Check out [action in PR](https://github.com/erikmunkby/dbt-changed-models/pull/3)

## 🚀 Usage

```yaml
jobs:
  dbt-diff:
    runs-on: ubuntu-latest

    steps:
      - uses: erikmunkby/dbt-changed-models@v1
        id: diff
        with:
          dbt-dir: my_dbt_project
          python-packages: dbt-postgres
          dbt-target: dev

      - name: Echo changed models
        run: |
          echo "Changed models:"
          echo "${{ steps.diff.outputs.models }}"

      - name: Use select statement
        run: |
          dbt run --select "${{ steps.diff.outputs.select-statement }}"
```

## ⚙️ Inputs
| Name              | Description                                            | Required  | Default                      |
| ----------------- | ------------------------------------------------------ | --------- | ---------------------------- |
| `dbt-dir`         | Path to your dbt project directory                     | ✅ Yes    | –                            |
| `python-packages` | Space-separated list of packages (e.g. `dbt-postgres`) | ✅ Yes    | –                            |
| `dbt-target`      | dbt profile target to use                              | ✅ Yes    | –                            |
| `state-type`      | Either `modified` or `new`                             | ❌ No     | `modified`                   |
| `temp-dir`        | Temp directory to store `manifest.json`                | ❌ No     | `__dbt_state_check_temp_dir` |
| `dbt-target-dir`  | Directory where `manifest.json` is written by dbt      | ❌ No     | `target`                     |

## 📤 Outputs
| Name               | Description                                              |
| ------------------ | -------------------------------------------------------- |
| `models`           | A newline-separated list of changed models               |
| `select-statement` | A dbt `--select`-ready statement (includes dependencies) |

## 📝 Example Output
If models stg_customers and dim_orders are modified:

```
models:
stg_customers
dim_orders

select-statement:
+stg_customers+ +dim_orders+
```

## 🛠 Requirements
- Action assumes it runs in a Pull Request with a base and head ref
- Requires access to the dbt target directory to extract `manifest.json`