# Airflow Installation Guide

This guide will set up Airflow with Docker Compose for a local, quick-start environment, which is great for testing and learning. Let’s get started.

## Prerequisites

- **Docker:** Ensure Docker is installed on your system. You can download it from the official Docker website (<https://www.docker.com/get-started>) and follow the installation instructions for your operating system.
- **Docker Compose:** Docker Compose should come with Docker Desktop on Windows and macOS. For Linux, you might need to install it separately—check the Docker Compose installation guide if needed.
- **Basic Familiarity:** You should be comfortable with basic terminal commands and have a general understanding of Airflow concepts like DAGs, the scheduler, and the webserver.

## Instructions

- **Create a Project Directory**: First, create a directory to store your Airflow setup files. This will help keep everything organized.

```bash

mkdir airflow-docker
cd airflow-docker
```

- **Fetch the Official Docker Compose File:** Airflow provides a docker-compose.yaml file to quickly set up a local environment. Download it using the following command:

```bash
curl -LfO '<https://airflow.apache.org/docs/apache-airflow/stable/docker-compose.yaml>'
```

This file sets up multiple services, including the Airflow webserver, scheduler, worker, a PostgreSQL database, and Redis for CeleryExecutor.

- **Create Necessary Directories:** The docker-compose.yaml file expects certain directories to exist for DAGs, logs, plugins, and configuration. Create them with:

```bash
mkdir -p ./dags ./logs ./plugins ./config
```

These directories will be mounted into the containers, allowing you to add DAGs and view logs from your local machine.

- **Set the `AIRFLOW_UID` Environment Variable:** On Linux, you need to set the `AIRFLOW_UID` to match your host user ID to avoid permission issues with the mounted directories. Run:

```bash
echo -e "AIRFLOW_UID=$(id -u)" > .env
```

If you're on Windows or macOS, you might see a warning about AIRFLOW_UID not being set, but you can ignore it for now.

- **Initialize the Airflow Database:** Before starting Airflow, you need to initialize the database and create an admin user. Run:

```bash
docker compose up airflow-init
```

This command will:

- Set up the PostgreSQL database.
- Run migrations.
- Create a default admin user with the username `airflow` and password `airflow`.

You should see output like:

```bash
airflow-init_1 | Upgrades done
airflow-init_1 | Admin user airflow created
airflow-init_1 | 2.x.x start_airflow-init_1 exited with code 0
```

- **Start All Airflow Services:** Now, start all the services defined in the `docker-compose.yaml` file:

```bash
docker compose up -d
```

The -d flag runs the containers in the background. This will start:

- `airflow-webserver`: The web interface at `http://localhost:8080`
- `airflow-scheduler`: Monitors and triggers tasks.
- `airflow-worker`: Executes tasks.
- `postgres`: The database.
- `redis`: For CeleryExecutor.

- **Verify the Containers Are Running:** Check that all containers are up and healthy:

```bash
docker ps
```

You should see several containers running, such as `airflow-webserver`, `airflow-scheduler`, `airflow-worker`, `postgres`, and `redis`. If any container is in an unhealthy state, there might be a resource issue—ensure Docker has at least 4GB of memory allocated (especially on macOS or Windows).

- **Access the Airflow Web Interface:** Open your browser and navigate to: `http://localhost:8080`

  - Log in with the default credentials:
        Username: airflow
        Password: airflow

You should now see the Airflow UI, where you can explore example DAGs or start creating your own.

## Adding Your Own DAGs

To add custom DAGs:

- Place your DAG Python files in the dags directory you created (`./dags`). The container will automatically sync this directory, and your DAGs should appear in the web interface after a short delay.

  - For example, create a file `dags/my_dag.py` with a simple DAG:

    ```python

    from airflow import DAG
    from airflow.operators.dummy import DummyOperator
    from datetime import datetime

    with DAG('my_dag', start_date=datetime(2025, 1, 1), schedule_interval=None) as dag:
        task1 = DummyOperator(task_id='task1')
    ```

- Refresh the Airflow UI, and you should see my_dag listed.

## Stopping and Cleaning Up

- To stop the containers:

```bash
docker compose down
```

- To stop and remove all containers, volumes, and images (a full reset):

```bash
docker compose down --volumes --rmi all
```

This is useful if you encounter issues and want to start fresh.

## Troubleshooting Tips

- **Memory Issues:** If the webserver keeps restarting, ensure Docker has enough memory (at least 4GB, ideally 8GB). On Docker Desktop, you can adjust this in the settings.
- **Port Conflicts:** If port 8080 is already in use, edit the docker-compose.yaml file to map to a different port (e.g., change 8080:8080 to 8081:8080).
- **Permission Issues:** If you see permission errors on Linux, double-check the AIRFLOW_UID in the .env file matches your user ID (id -u).
- **Logs:** Check container logs for debugging:

```bash
docker logs <container_name>
```

## Customizing the Setup

If you need to install additional Python packages or system libraries:

- Create a `Dockerfile` to extend the Airflow image:

```dockerfile
FROM apache/airflow:latest
USER airflow
COPY requirements.txt .
RUN pip install -r requirements.txt
```

- Create a `requirements.txt` with your dependencies, e.g.:

```python
pandas
numpy
...
...
```

- Modify the `docker-compose.yaml` to use your custom image by adding a build section for the `airflow-webserver`, `airflow-scheduler`, and `airflow-worker` services. For example:

```yaml
services:
  airflow-webserver:
    build:
      context: .
      dockerfile: Dockerfile
    # ... rest of the configuration
```

- Rebuild and restart:

```bash
docker compose up --build -d
```
