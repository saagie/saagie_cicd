# Saagie DataOps Platform CI/CD GitHub Action

This GitHub Action is designed to streamline the CI/CD process for the Saagie Platform.
It provides a set of customizable options to enhance your jobs and pipelines on the Saagie platform.


## Usage


> **_Before you begin:_**
> - You must have a Saagie account with at least editor rights to the platform(s). For more information on access rights, see [About Groups Page](https://docs.saagie.io/user/latest/data-team/security-module/about-security-module#security-groups). 
> - You must have at least one project on your Saagie platform(s). For more information on how to create projects, see [Creating Projects](https://docs.saagie.io/user/latest/data-team/projects-module/projects/managing-projects#projects-create).

1. Create the following environment variables:
    - `SAAGIE_URL`: This is the URL of your Saagie platform.
    - `SAAGIE_USER`: This is the username of your Saagie account.
    - `SAAGIE_PASSWORD`: This is the password of your Saagie account.
    - `SAAGIE_REALM`: This is the [DNS prefix](https://docs.saagie.io/user/latest/admin/installation/system-requirements#install-sysreq-dns-entry) determined when Saagie was installed. The Realm allows you to connect to your Saagie platform.

     They will be used to make the connection with your Saagie platform. For more information on how to create environment variables, you can see:

      | Environment | Documentation Link |
      | :----: | :---- |
      | GitHub | [Creating configuration variables for a repository](https://docs.github.com/en/actions/learn-github-actions/variables#creating-configuration-variables-for-a-repository). <br> :memo: Note: If you have password type variables, you must create them as secrets. |
      | Linux | [Environment Variables](https://wiki.debian.org/EnvironmentVariables) |
      | Windows | [Create and Modify Environment Variables on Windows](https://docs.oracle.com/en/database/oracle/machine-learning/oml4r/1.5.1/oread/administrative-tasks-oracle-machine-learning-r.html#GUID-DD6F9982-60D5-48F6-8270-A27EC53807D0) |
      | Mac | [Use environment variables in Terminal on Mac](https://support.apple.com/en-gb/guide/terminal/apd382cc5fa-4f58-4449-b20a-41c53c006f8f/2.13/mac/13.0) |

1. Import the `cicd_saagie_tool` folder in your work directory, which includes the `__main__.py`, `requirements.txt`, and `utils.py` files.
1. Create a GitHub Action workflow file in your repository. For more information, see the GitHub documentation for [Using workflows](https://docs.github.com/en/actions/using-workflows#creating-a-workflow-file).
1. Add the following step to your workflow file:
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
              saagie_env: 'dev' # Environment to connect to the Saagie Platform. It should be the same as the one in the env config files.
              job_config_folder: 'saagie/jobs/*.json'  # Folder where job config files are stored
              pipeline_config_folder: 'saagie/pipelines/*.json'  # Folder where pipeline config files are stored
              env_config_folder: './saagie/envs/*.json'  # Folder where env config files are stored
              job_source_folder: './code/jobs/*/*'  # Folder where job source code files are stored
              pipeline_source_folder: './code/pipelines/*.yaml'  # Folder where pipeline source code files are stored
              artefact_code_folder: './dist/*/*'  # Folder where artefact code files are stored
    ```
    Where the path to configuration and code files can be replaced with your own value.  
      
     > :memo: Note: When using the `package_job` action, the zip file will be stored in the `{artefact_code_folder}/{job_name}` folder. The name of the zip file will be `{job_name}.zip`.
1. Make sure you have the required configuration and code files for each Saagie platform, job, and pipeline. For more information, see our [template repository](https://github.com/saagie/template_cicd/tree/main).

   > :memo: Note: The configuration files are stored in the `saagie` directory. The code files are stored in the `code` directory.
   
   > :memo: Note: For more information on the set up process, see [CI/CD With Saagie Python API](https://docs.saagie.io/user/latest/developer/ci-cd/).

## Inputs

| Inputs | Description |
| :----: | :---- |
| `action` <br> (optional) | What you want to do on your Saagie platform. <br> The available values are: `package_job`, `update_job`, `update_pipeline`, `update`. <br> The default value is: `update`. |
| `saagie_url` <br> (optional) | The URL of your Saagie platform. |
| `saagie_user` <br> (optional) | The username of your Saagie account. |
| `saagie_pwd` <br> (optional) | The password of your Saagie account. |
| `saagie_realm` <br> (optional) | The DNS prefix determined when Saagie was installed. The Realm allows you to connect to your Saagie platform. |
| `saagie_env` <br> (optional) | The Saagie platform you want to connect to. <br> Example: dev, prod, preprod, etc. |
| `debug_mode` <br> (optional) | Debug mode. It is deactivated by default. <br> Set something other than `' '` to enable the debug mode. |
| `job_config_folder` <br> (optional) | The folder where the job configuration files are stored. <br> The default location is: `'saagie/jobs/*.json'`. |
| `pipeline_config_folder` <br> (optional) | The folder where the pipeline configuration files are stored. <br> The default location is: `'saagie/pipelines/*.json'`. |
| `env_config_folder` <br> (optional) | The folder where the environment configuration files are stored. <br> The default location is: `'./saagie/envs/*.json'`. |
| `job_source_folder` <br> (optional) | The folder where the job source code files are stored. <br> The default location is: `'./code/jobs/*/*'`. |
| `pipeline_source_folder` <br> (optional) | The folder where the pipeline source code files are stored. <br> The default location is: `'./code/pipelines/*.yaml'`. |
| `artefact_code_folder` <br> (optional) | The folder where the artefact code files are stored. <br> The default location is: `'./dist/*/*'`. |

## Example

```yaml
with:
  action: 'update'
  saagie_url: ${{ secrets.SAAGIE_URL }}
  saagie_user: ${{ secrets.SAAGIE_USER }}
  saagie_pwd: ${{ secrets.SAAGIE_PASSWORD }}
  saagie_realm: ${{ secrets.SAAGIE_REALM }}
  saagie_env: dev
  job_config_folder: 'saagie/jobs/*.json'
  pipeline_config_folder: 'saagie/pipelines/*.json'
  env_config_folder: './saagie/envs/*.json'
  job_source_folder: './code/jobs/*/*'
  pipeline_source_folder: './code/pipelines/*.yaml'
  artefact_code_folder: './dist/*/*'
```

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
