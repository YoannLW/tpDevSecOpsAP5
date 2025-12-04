# DevOps Final Project

This project consists of a Go backend, a static frontend served by Nginx, and a PostgreSQL database. It is containerized using Docker and orchestrated with Docker Compose.

## Architecture

- **Frontend**: HTML/CSS/JS served by Nginx.
- **Backend**: Go application exposing a REST API.
- **Database**: PostgreSQL.

## Prerequisites

- Podman
- Podman Compose

## Configuration

Create a `.env` file in the root directory with the following content:

```env
POSTGRES_USER=postgres
POSTGRES_PASSWORD=clicker
POSTGRES_DB=clicker
```

## Running Locally

1. Clone the repository.
2. Run the following command to start the application:

```bash
podman-compose up -d --build
```

3. Access the application:
    - Frontend: [http://localhost](http://localhost)
    - Backend Health Check: [http://localhost:8081/health](http://localhost:8081/health)

## Git Flow

We follow a simplified Git Flow:

- **main/master**: Production-ready code.
- **feature/*** branches: For new features and bug fixes.

### Workflow

1. Create a new branch for your feature: `git checkout -b feature/my-feature`.
2. Make changes and commit.
3. Push the branch and open a Pull Request.
4. CI/CD pipeline runs security checks.
5. Upon approval and merge to `main`, the application is automatically deployed.

## CI/CD Pipeline

The CI/CD pipeline is implemented using GitHub Actions and includes:

- **Security Checks**:
    - **TruffleHog**: Scans for secrets.
    - **Trivy**: Vulnerability scanner for filesystem and dependencies.
    - **Gosec**: Security linter for Go code.
- **Deployment**:
    - Triggers a webhook to deploy the application to Coolify.

## Deployment

The application is designed to be deployed on **Coolify**.

### 1. Configure Coolify
1.  Log in to your Coolify instance (e.g., `https://coolify.niceapp.cloud/`).
2.  Create a new **Project** and **Environment**.
3.  Add a new **Resource** > **Git Repository**.
4.  Select this repository (`YoannLW/tpDevSecOpsAP5`).
5.  **Configuration**:
    *   **Build Pack**: Select **Docker Compose**.
    *   **Domains**:
        *   For the `frontend` service, set the domain to: `http://tpfinal.<your-domain>.store`.
        *   (Optional) For the `backend` service, you can set a domain or leave it internal.
6.  **Environment Variables**:
    *   Go to the **Environment Variables** tab in your Coolify resource.
    *   Add the following variables (keys and values):
        *   `POSTGRES_USER`: `postgres` (or your chosen user)
        *   `POSTGRES_PASSWORD`: `clicker` (or your chosen password)
        *   `POSTGRES_DB`: `clicker` (or your chosen db name)
    *   **Save** the variables.
    *   **Redeploy** the application.

### 2. Setup CI/CD Webhook
1.  In your Coolify resource, go to **Webhooks**.
2.  Copy the **Deploy Webhook** URL (curl command).
3.  Go to your GitHub Repository > **Settings** > **Secrets and variables** > **Actions**.
4.  Create a new Repository Secret named `COOLIFY_WEBHOOK`.
5.  Paste the full webhook URL (e.g., `https://coolify.../api/v1/deploy...`).

### 3. Deploy
*   **Automatic**: Pushing to the `main` branch will trigger the GitHub Action, which runs security checks and then calls the Coolify webhook to deploy.
*   **Manual**: You can also click "Deploy" directly in the Coolify UI.