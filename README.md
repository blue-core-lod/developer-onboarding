# Blue Core Developer On-boarding

This repository contains some documentation and configuration to help you get started with the [Blue Core](https://bluecore.info/) stack. Blue Core aims to create a community-operated BIBFRAME datastore where ownership and creation of the metadata are shared among member institutions, eliminating the need for duplicative institutional copies, and bringing library linked open data to production at scale. The technical stack currently includes:

* BIBFRAME Editors: [Marva](https://github.com/blue-core-lod/marva_editor) and 
  [Sinopia](https://github.com/blue-core-lod/sinopia_editor) are included in the stack to demonstrate 
  how the core Blue Core service can serve as a storage system used by a larger ecosystem of BIBFRAME tools.
* API Service: A REST based API for creating, reading, updating and deleting BIBFRAME data. It also 
  includes an MCP API for generative AI agents. Built with [FastAPI](https://fastapi.tiangolo.com/)
* Identity Management: A [Keycloak](https://www.keycloak.org/) based system for managing human and automated 
  access to the API.
* Workflow System: An [Apache Airflow](https://airflow.apache.org/) system for managing the loading BIBFRAME 
  data into ILS systems and ingesting CBD files into Blue Core Postgres database.
* A developer account is available for use in the various services:
  - **user:** `developer`
  - **password:** `123456`
Below are some instructions for getting started experimenting with you own instance of these services.

## Local Setup
Steps for setting up the Blue Core stack to run locally on your machine.

1. Clone the repository: `git clone --recurse-submodules https://github.com/blue-core-lod/developer-onboarding.git`
2. Change to the `developer-onboarding` directory
3. Run `cp .env-example .env` for local environment file.
4. Build the Marva editor image with the local Keycloak URL:
   ```
   docker build \
     --build-arg VITE_KEYCLOAK_AUTH_PATH="http://localhost:8000/keycloak/" \
     --build-arg VITE_BLUECORE_API_PATH="http://localhost:8000/api/" \
     --tag marva:local \
     "https://github.com/blue-core-lod/marva_editor.git#onboarding"
   ```
   > **Note:** This step is required because the Marva editor is a Vite app that bakes environment variables (including the Keycloak URL) into the static bundle at build time.

5. Build the Sinopia editor image with the local Keycloak URL:
   ```
   docker build \
     --build-arg KEYCLOAK_URL="http://localhost:8000/keycloak/" \
     --build-arg SINOPIA_URI="http://localhost:8000/sinopia" \
     --build-arg SINOPIA_API_BASE_URL="http://localhost:8000/api" \
     --build-arg SEARCH_HOST="http://localhost:8000/api/search" \
     --tag sinopia:local \
     "https://github.com/blue-core-lod/sinopia_editor.git"
   ```

6. Start Docker environment `docker compose up -d`

7. Set-up Blue Core Airflow
   1. Click on the `http://localhost:8000/workflows`
   2. Add `bluecore_url` variable with value of `http://localhost:8000`
   3. Unpause `resource_loader` DAG 

## Install Dependencies with uv
1. If you haven't already, please install [uv](https://github.com/astral-sh/uv)
2. Run `uv sync` to install the dependencies

## Creating a custom user/group in the Keycloak Bluecore realm

## Launch Jupyter Lab
From the `developer-onboarding` folder, launch [Jupyter Lab](https://jupyter.org/) with the following 
command:

`uv run jupyter lab`

## Ingesting Sinopia Profiles
1. Click on the `notebooks` folder
2. Open the `01_UploadSinopiaProfiles.ipynb` notebook
3. Run all of the cells in the notebook.

## Ingesting CBD files 

(with the participants find CBDs in id.loc.gov), either through the command-line, the Graph Toolbox, or a Jupyter Notebook)

### Loading 50 sample CBD files for a Jupyter Notebook
1. Launch the `02_Load_CBDs.ipynb` notebook
2. Run all of the cells in the notebook
3. This code will launch 50 runs of the `resource_loader` DAG

### Create Work and Instance Vectors
1. Make sure all of the `r
1. Launch the `03_WorkAndInstanceVectors.ipynb`
2. Run the cells

## Editing in Sinopia
1. Log into Sinopia 
2. Search for an existing Work or Instance
3. When loading, select either a Blue Core `_Work` or `_Instance` template
4. Add a note in Sinopia
5. Save (and fix any errors) 
6. Directly open the resource with the URL to see the changes

## Editing in Marva
1. Open Marva
2. Copy a Instance URL into Load field
3. Edit the Loaded Instance or Work
4. POST to save the changes to the Blue Core 

## Using the Graph Toolbox
1. Local edit `nginx/toolbox/index.html` on line 49, change to use port 8000
2. Click on Graph toolbox
3. Run a search
4. Load resources

## Using Blue Core AI Agent(s) in a Jupyter notebook
1. Set 'PROVIDER_URL' and 'PROVIDER_API_KEY' for OpenAI or Antropic Client
2. Launch the `04_DeDuplicateAgent.ipynb` notebook
3. Run the cells and experiment with results
 

## Using the MCP API with Claude Code
1. Use Claude Code
2. Give Claude a prompt to `Use the local Blue Core MCP server using notebooks/blue_core_mcp.py` 
