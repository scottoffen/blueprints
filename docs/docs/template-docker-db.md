---
sidebar_position: 4
title: Docker Database Templates
---

Running a database in Docker is one of the fastest ways to get a local development
environment up and running without installing anything on the host machine. This
template generates a `docker-compose.yaml` pre-configured for your chosen database
provider, complete with data persistence, health checks, and automatic restarts.

## Docker Database (`dockerdb`)

Adds a `docker-compose.yaml` configured for a database provider to the current
directory.

### Parameters

| Parameter | Required | Default | Description |
|-----------|----------|---------|-------------|
| `--provider` | No | `postgres` | The database provider. One of `postgres`, `mysql`, or `mssql`. |
| `--dbname` | No | `appdb` | The name of the database. |
| `--dbuser` | No | `<dbname>_user` | The database username. Defaults to the database name with `_user` appended. |
| `--dbpwd` | No | `StrongPassword123!` | The database password. Must be at least 8 characters. |
| `--dbport` | No | Provider default | The host port to expose. Defaults to the standard port for the selected provider: `5432` for PostgreSQL, `3306` for MySQL, and `1433` for SQL Server. |

### Usage

```bash
dotnet new dockerdb
```

```bash
dotnet new dockerdb --provider mysql --dbname myapp --dbport 3307
```

### Output

```
docker-compose.yaml
```

### What Gets Generated

All three providers share a common structure:

- **Project name:** set to the value of `--name`, which Docker uses as a namespace
  for containers, networks, and volumes. This must be unique per project to avoid
  database collisions between running containers.
- **Data persistence:** a named `dbdata` volume stores the database files. Docker
  prefixes it with the project name, so volumes are automatically namespaced per
  project.
- **Port mapping:** exposes the database to the host machine so tools like DBeaver,
  SSMS, MySQL Workbench, or `psql` can connect via `localhost`.
- **Restart policy:** set to `unless-stopped`, so the container restarts
  automatically after crashes or Docker daemon restarts.
- **Health check:** each provider uses the appropriate mechanism to confirm the
  database is ready to accept connections before dependent services start.

### Starting and Stopping

**Starting**

Run the following command from the directory containing `docker-compose.yaml` to
start the container in detached mode:

```bash
docker compose up -d
```

The `-d` flag runs the container in the background, returning control to the terminal
immediately rather than attaching to the container's output stream.

**Stopping**

To stop the container without removing any data:

```bash
docker compose down
```

To stop the container and remove the data volume:

```bash
docker compose down -v
```

The `-v` flag removes the named volumes defined in the compose file. Use this when
you want to reset the database to a clean state, such as when reinitializing with
different credentials or starting fresh for a new test run.

:::danger
This action is irreversible. All data stored in the volume will be permanently deleted.
:::

**PostgreSQL**

Environment variables are used only during the first initialization of the database.
If the data volume already exists, changing these values will have no effect. To
change credentials after initialization, alter them inside the database directly or
reset the volume.

**MySQL**

Behaves the same as PostgreSQL with respect to initialization. Environment variables
are applied only on first run. The root password is set to the same value as the
database password.

**Microsoft SQL Server**

SQL Server differs from the other providers in one important way: it does not create
a database or user automatically on startup. This template includes a second
one-time service, `db_init`, that waits for SQL Server to be ready and then creates
the database and user if they do not already exist. The SQL Server password must meet
complexity requirements. The minimum length is 8 characters.