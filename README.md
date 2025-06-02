# SQL Server Docker Compose Setup

> **⚠️ WARNING:**
> The host directory specified by `CONTAINER_FOLDER` (used for data persistence) **must be accessible by user and group ID `10001`**, which is the user created inside the container to run SQL Server. If the directory permissions are not set correctly, SQL Server will fail to start or may not be able to write data. Ensure the folder exists and is owned by UID and GID `10001`:
>
> ```bash
> sudo mkdir -p /your/host/folder/sql-server
> sudo chown -R 10001:10001 /your/host/folder/sql-server
> ```
> Replace `/your/host/folder` with your actual path.

This project provides a ready-to-use `docker-compose.yml` for running Microsoft SQL Server in a containerized environment. It is ideal for development, testing, and local database management.

## Features
- **Microsoft SQL Server**: Uses the official Microsoft SQL Server image.
- **Configurable Password**: Set your own SA password via environment variable.
- **Persistent Storage**: Database files are stored on your host for data persistence.
- **Healthcheck**: Ensures the SQL Server instance is running and healthy.

## Prerequisites
- [Docker](https://www.docker.com/get-started)
- [Docker Compose](https://docs.docker.com/compose/install/)

## Usage

### Environment Variables
The following environment variables are used in `docker-compose.yml`:

- `SA_PASSWORD` (**required**): The password for the SQL Server system administrator (SA) account. Must meet SQL Server password requirements (at least 8 characters, including uppercase, lowercase, numbers, and symbols).
- `CONTAINER_FOLDER` (**optional**): The base folder on your host machine where SQL Server data will be persisted. Defaults to `container-folder` if not set. The full path used by Docker will be `/${CONTAINER_FOLDER}/sql-server`.

Create a `.env` file in this directory with the following content:
```env
SA_PASSWORD=YourStrong!Passw0rd
CONTAINER_FOLDER=your/host/folder
```
- Replace `YourStrong!Passw0rd` with a strong password.
- Set `CONTAINER_FOLDER` to your preferred host directory for persistent data (or leave blank to use the default `container-folder`).

#### Volumes
The following volume is mounted in the container:
- `/${CONTAINER_FOLDER}/sql-server:/var/opt/mssql`: Persists all SQL Server data files on your host. This ensures your database data is not lost when the container is removed or recreated.

1. **Set Environment Variables**
   
   Create a `.env` file in this directory with the following content:
   ```env
   SA_PASSWORD=YourStrong!Passw0rd
   CONTAINER_FOLDER=your/host/folder
   ```
   - Replace `YourStrong!Passw0rd` with a strong password (required by SQL Server).
   - Set `CONTAINER_FOLDER` to your preferred host directory for persistent data (or leave blank to use the default `container-folder`).

2. **Start SQL Server**
   ```bash
   docker-compose up -d
   ```

3. **Connect to SQL Server**
   - **Host (from host machine):** `localhost`
   - **Host (from another container):** `sqlserver`
   - **Port:** `1433`
   - **User:** `SA`
   - **Password:** (as set in `.env`)

   You can use tools like [Azure Data Studio](https://docs.microsoft.com/en-us/sql/azure-data-studio/download-azure-data-studio) or `sqlcmd` to connect.

4. **Stop SQL Server**
   ```bash
   docker-compose down
   ```

## Notes
- The container will restart automatically unless stopped.
- The container hostname is set to `sqlserver` (see `container_name` in `docker-compose.yml`).
- The healthcheck uses the default password from the example. If you change the SA password, update the healthcheck command in `docker-compose.yml` accordingly.
- For production use, review and harden security settings.

## License
This project is licensed under the MIT License.
