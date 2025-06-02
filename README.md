# SQL Server Docker Compose Setup

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
- `DATA_FOLDER` (**required**): The absolute path on your host machine where SQL Server data will be persisted. This ensures your database files are not lost when the container is removed.

Create a `.env` file in this directory with the following content:
```env
SA_PASSWORD=YourStrong!Passw0rd
DATA_FOLDER=your/data/folder/path
```
- Replace `YourStrong!Passw0rd` with a strong password.
- Set `DATA_FOLDER` to an absolute path on your host for data persistence.

1. **Set Environment Variables**
   
   Create a `.env` file in this directory with the following content:
   ```env
   SA_PASSWORD=YourStrong!Passw0rd
   DATA_FOLDER=your/data/folder/path
   ```
   - Replace `YourStrong!Passw0rd` with a strong password (required by SQL Server).
   - Set `DATA_FOLDER` to an absolute path on your host for data persistence.

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
