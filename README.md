# Blue Core Developer On-boarding

This repository contains some documentation and configuration to help you get started with the [Blue Core](https://bluecore.info/) stack. Blue Core aims to create a community-operated BIBFRAME datastore where ownership and creation of the metadata are shared among member institutions, eliminating the need for duplicative institutional copies, and bringing library linked open data to production at scale. The technical stack currently includes:

* BIBFRAME Editors: Marva and Sinopia are included in the stack to demonstrate how the core Blue Core service can serve as a storage system used by a larger ecosystem of BIBFRAME tools.
* API Service: A REST based API for creating, reading, updating and deleting BIBFRAME data. It also includes an MCP API for generative AI agents. 
* Identity Management: A Keycloak based system for managing human and automated access to the API.
* Workflow System: An Apache Airflow system for managing the loading BIBFRAME data into ILS systems.

Below are some instructions for getting started experimenting with you own instance of these services.

## Local Setup
Steps for setting up the Blue Core stack to run locally on your machine.

1. Clone the repository: `git clone --recurse-submodules https://github.com/blue-core-lod/developer-onboarding.git`
2. Change to the `developer-onboarding` directory
3. Build the Marva editor image with the local Keycloak URL:
   ```
   docker build \
     --build-arg VITE_KEYCLOAK_AUTH_PATH="http://localhost:8000/keycloak/" \
     --build-arg VITE_BLUECORE_API_PATH="http://localhost:8000/api/" \
     --tag marva:local \
     "https://github.com/blue-core-lod/marva_editor.git#onboarding"
   ```
   > **Note:** This step is required because the Marva editor is a Vite app that bakes environment variables (including the Keycloak URL) into the static bundle at build time.
4. Build the Sinopia editor image with the local Keycloak URL:
   ```
   docker build \
     --build-arg KEYCLOAK_URL="http://localhost:8000/keycloak/" \
     --build-arg SINOPIA_URI="http://localhost:8000/sinopia" \
     --build-arg SINOPIA_API_BASE_URL="http://localhost:8000/api" \
     --build-arg SEARCH_HOST="http://localhost:8000/api/search" \
     --tag sinopia:local \
     "https://github.com/blue-core-lod/sinopia_editor.git"
   ```
5. Start Docker environment `docker compose up -d`
6. Run Alembic Database Migrations
   ```
    docker run --rm -it --network host python:3.12 bash -c "
        git clone https://github.com/blue-core-lod/bluecore-models.git &&
        cd bluecore-models &&
        curl -LsSf https://astral.sh/uv/install.sh | sh &&
        source /root/.local/bin/env &&
        uv sync &&
        uv run alembic upgrade head
      "
   ```
6. Set-up Blue Core Airflow
   1. Add `bluecore_url` variable with value of `http://localhost:8000`
   2. Unpause `resource_loader` DAG 

## Creating a custom user/group in the Keycloak Bluecore realm

## Ingesting CBD files 

(with the participants find CBDs in id.loc.gov), either through the command-line, the Graph Toolbox, or a Jupyter Notebook)

## Editing in Sinopia

## Editing in Marva

## Using the Graph Toolbox

## Using Blue Core AI Agent(s) in a Jupyter notebook

## Using the MCP API

