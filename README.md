# Blue Core Developer On-boarding


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
     "https://github.com/blue-core-lod/marva_editor.git#bluecore-dev"
   ```
   > **Note:** This step is required because the Marva editor is a Vite app that bakes environment variables (including the Keycloak URL) into the static bundle at build time.
4. Build the Sinopia editor image with the local Keycloak URL:
   ```
   docker build \
     --build-arg KEYCLOAK_URL="http://localhost:8000/keycloak/" \
     --build-arg SINOPIA_URI="http://localhost:8000/sinopia" \
     --build-arg SINOPIA_API_BASE_URL="http://localhost:8000/api" \
     --build-arg SEARCH_HOST="http://localhost:8000/api" \
     --tag sinopia:local \
     "https://github.com/blue-core-lod/sinopia_editor.git"
   ```
4. Start Docker environment `docker compose up -d`
