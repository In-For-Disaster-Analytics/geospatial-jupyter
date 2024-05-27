# Cookbook repository for Conda environments

This repository contains the files needed to build a Docker image with a Conda environment for running the Tapis apps.

This Docker image is specifically configured to install a singular Conda environment, based on the contents of a Git repository. The repository must contain a `./binder/environment.yml` file with the Conda environment definition, and optionally a `./binder/requirements.txt` file with additional Python dependencies.

## Installation process

When the job is submitted, the Tapis platform will pull the Docker image and run the container. The container will run the `run.sh` script, which will activate the Conda environment and run the app.

## Important Variables

- `GIT_REPO_URL`: URL of the cookbook repository to use.
- `GIT_BRANCH`: Branch of the cookbook repository to use.
- `COOKBOOK_NAME`: Name of the cookbook, used for naming the directory
- `COOKBOOK_WORKSPACE_DIR`: This is the designated directory for cloning the cookbook repository. If the directory already exists, the script does not perform an update. This directory serves as the active working directory during the Jupyter session, allowing direct interaction with the contents.
- `COOKBOOK_REPOSITORY_DIR`: This directory is also used for cloning the cookbook repository. However, in contrast to `COOKBOOK_WORKSPACE_DIR`, if it already exists, the script updates the repository to ensure it contains the most recent changes. This directory is maintained as a hidden area, primarily utilized for update checks and not intended for direct user interaction.
- `COOKBOOK_CONDA_ENV`: Name of the Conda environment to use.

## Execution

1. `install_conda`: Checks if Miniconda is installed in a specified directory (`$WORK/miniconda3`). If not installed, it downloads and installs Miniconda, configures the `PATH`, and sets Conda to not automatically activate the base environment on startup.
2. `load_cuda`: Loads the CUDA module, version 12.0
3. `export_repo_variables`: Sets and exports various environment variables related to a Git repository, its environment. Set up the repository and branch to use, and the environment name.
4. `clone_repository`:
   6.1 Clones the Git repository specified in `GIT_REPO` and `GIT_BRANCH` into the directory specified in `COOKBOOK_WORKSPACE_DIR`. If the directory exists, it doesn't clone the repository or update it.
   6.2. Clone the Git repository specified in `GIT_REPO` and `GIT_BRANCH` into the directory specified in `COOKBOOK_REPOSITORY_DIR`. If the directory exists, it updates the repository.
5. `load_tap_functions`: Loads TACC's specific functions for job management.
6. `get_tap_certificate`: Ensures a TLS certificate exists for a secure session.
7. `get_tap_token`: Generates a token for a Jupyter session and retrieves a port for login.
8. `create_jupyter_configuration`: Creates a configuration file for a JupyterLab session, including SSL options and kernel settings.
9. `run_jupyter`: Starts JupyterLab and logs its output. It retries once if the first attempt fails.
10. `port_fowarding`: Sets up SSH tunneling for port forwarding, allowing external access to the JupyterLab session.
11. `send_url_to_webhook`: Sends the JupyterLab session URL to a webhook, presumably to notify users that the session is ready.
12. `session_cleanup`: Monitors a file (`delete_me_to_end_session`) and ends the Jupyter session when this file is deleted.
13. `install_dependencies`: Creates or activates a Conda environment specified in the Git repository and installs necessary Python dependencies.

## Auxiliary Functions

1. `detect_update_available`: Checks if there's an update available for the Git branch specified in `GIT_BRANCH`. If there is, it creates or updates a file indicating this.
2. `remove_update_available_file`: Deletes a `UPDATE_AVAILABLE.txt` which indicates an available update if it exists.

## Files

Production files:

- `app.json` Tapis app definition file.
- `Dockerfile`: Dockerfile for building the image prepared to use GPUs.
- `run.sh` Script to run in the container.

Development files:

- `cpu/Dockerfile.cpu`: Dockerfile for building the image prepared to use CPUs.
- `run.sh` Script to run in the container.
- `app-dev.json` Tapis app definition file for development.
