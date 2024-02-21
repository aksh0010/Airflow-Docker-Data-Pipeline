# Docker Application with Airflow

This is a simple demonstration of how data can be shared among tasks in an Airflow DAG. The application uses Docker to containerize the services.

## Application Structure

The application consists of two main services: `postgres` and `webserver`.

### Postgres

The `postgres` service uses the `postgres:9.6` image and is configured with the following environment variables:

- `POSTGRES_USER=airflow`
- `POSTGRES_PASSWORD=airflow`
- `POSTGRES_DB=airflow`

### Webserver

The `webserver` service is built from the Dockerfile located in `./dockerfiles`. It depends on the `postgres` service and has the following environment variables:

- `LOAD_EX=n`
- `EXECUTOR=Local`

The `webserver` service also exposes port `8080` and mounts the `./dags` directory to `/usr/local/airflow/dags` in the container.

## Airflow DAG

The Airflow DAG (`first_dag`) consists of two tasks: `first_function_execute` and `second_function_execute`. The `first_function_execute` task pushes a value to XCom, which is then pulled by the `second_function_execute` task.

## Building the Application

To build the application, you need to have Docker installed. Once Docker is installed, you can build and run the application using the following command:

```bash
docker-compose up --build
