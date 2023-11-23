# Saagie Platform CICD GitHub Action

This GitHub Action is designed to streamline the continuous integration and continuous deployment (CICD)
process for the Saagie Platform. 
It provides a set of customizable options to upgrade jobs and/or pipelines on the Saagie Platform.

## Usage

```yaml
name: Saagie CICD

on:
  push:
    branches:
      - main

jobs:
  saagie-cicd:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Repository
        uses: actions/checkout@v2

      - name: Saagie CICD Action
        uses: <repository>/<action-name>@<release-tag>
        with:
          action: 'update'  # Available values: package_job, update_job, update_pipeline, update. Default: update
          saagie_url: ${{ secrets.SAAGIE_URL }}
          saagie_user: ${{ secrets.SAAGIE_USER }}
          saagie_pwd: ${{ secrets.SAAGIE_PASSWORD }}
          saagie_realm: ${{ secrets.SAAGIE_REALM }}
          saagie_env: ${{ secrets.SAAGIE_ENV }}
          debug_mode: 'true'  # Optional, deactivated by default, if you set to something different of '' then it activates debug mode
          job_config_folder: 'saagie/jobs/*.json'  # Folder where job config files are stored
          pipeline_config_folder: 'saagie/pipelines/*.json'  # Folder where pipeline config files are stored
          env_config_folder: './saagie/envs/*.json'  # Folder where env config files are stored
          job_source_folder: './code/jobs/*/*'  # Folder where job source code files are stored
          pipeline_source_folder: './code/pipelines/*.yaml'  # Folder where pipeline source code files are stored
          artefact_code_folder: './dist/*/*'  # Folder where artefact code files are stored
```

## Inputs

### `action` (optional)

What you want to do with Saagie Platform. Available values: `package_job`, `update_job`, `update_pipeline`, `update`. 
Default value: `update`

### `saagie_url` (optional)

URL of the Saagie Platform.

### `saagie_user` (optional)

User to connect to the Saagie Platform.

### `saagie_pwd` (optional)

Password to connect to the Saagie Platform.

### `saagie_realm` (optional)

Realm to connect to the Saagie Platform.

### `saagie_env` (optional)

Environment to connect to the Saagie Platform.

### `debug_mode` (optional)

Debug mode. Set to 'true' for debug mode.

### `job_config_folder` (optional)

Folder where the job config files are stored. Default: 'saagie/jobs/*.json'

### `pipeline_config_folder` (optional)

Folder where the pipeline config files are stored. Default: 'saagie/pipelines/*.json'

### `env_config_folder` (optional)

Folder where the env config files are stored. Default: './saagie/envs/*.json'

### `job_source_folder` (optional)

Folder where the job source code files are stored. Default: './code/jobs/*/*'

### `pipeline_source_folder` (optional)

Folder where the pipeline source code files are stored. Default: './code/pipelines/*.yaml'

### `artefact_code_folder` (optional)

Folder where the artefact code files are stored. Default: './dist/*/*'

## Example

```yaml
with:
  action: 'update'
  saagie_url: ${{ secrets.SAAGIE_URL }}
  saagie_user: ${{ secrets.SAAGIE_USER }}
  saagie_pwd: ${{ secrets.SAAGIE_PASSWORD }}
  saagie_realm: ${{ secrets.SAAGIE_REALM }}
  saagie_env: ${{ secrets.SAAGIE_ENV }}
  job_config_folder: 'saagie/jobs/*.json'
  pipeline_config_folder: 'saagie/pipelines/*.json'
  env_config_folder: './saagie/envs/*.json'
  job_source_folder: './code/jobs/*/*'
  pipeline_source_folder: './code/pipelines/*.yaml'
  artefact_code_folder: './dist/*/*'
```

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.


