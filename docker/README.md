# Liferay DXP 7.4 Local Development Environment

This project provides a Docker Compose setup for running a local Liferay DXP 7.4 environment with MySQL. It is designed for development and testing purposes, following best practices for modular, maintainable Liferay projects.

## Services

- **lr-mysql**: MySQL 8.0 database for Liferay.
- **lr-liferay-1**: Liferay DXP 7.4.3.125-GA125 application server.
- *(Optional, commented out)*: Nginx reverse proxy and Elasticsearch 7.17.18 for advanced scenarios.

## Directory Structure

```
docker/
├── docker-compose.yaml
└── mnt/
    ├── liferay/
    │   ├── deploy/                # Hot deploy directory for modules
    │   ├── data/document_library/ # Liferay document library storage
    │   ├── osgi/
    │   │   ├── client-extensions/ # Client Extensions (CETs)
    │   │   └── modules/           # OSGi modules
    ├── mysql/
    │   └── data/                  # MySQL data volume
    └── ...                        # (Optional) nginx, elasticsearch, etc.
```

## Usage

### Prerequisites

- [Docker](https://docs.docker.com/get-docker/) and [Docker Compose](https://docs.docker.com/compose/) installed.

### Starting the Environment

From the `docker/` directory, run:

```bash
docker compose up -d
```

This will start the MySQL and Liferay containers in the background.

### Accessing Liferay

- Liferay Portal: [http://localhost:8081](http://localhost:8081)
- MySQL: `localhost:3307` (user: `root`, password: `root`)
- Liferay AJP Port: 8010
- Liferay GoGo Shell: 11312

### Stopping the Environment

```bash
docker compose down
```

## Volumes and Persistence

- **MySQL data** is persisted in `mnt/mysql/data`.
- **Liferay deploy, document library, modules, and client extensions** are mapped to `mnt/liferay/` subfolders for easy development and hot deployment.

## Database Configuration

MySQL is configured with the following settings:
- Database name: `lportal`
- Root password: `root`
- Port mappings:
  - 3307:3306 (MySQL)
  - 33070:33060 (MySQL X Protocol)

## Liferay Configuration

The Liferay container is configured with:
- JDBC connection settings for MySQL
- Document library persistence
- OSGi module deployment support
- Client extensions support
- Glowroot monitoring enabled

## Customization

### Optional Services

The following services are available but commented out in the docker-compose.yaml:
- **Nginx**: Reverse proxy (port 80)
- **Elasticsearch**: Search engine (ports 9210, 9310)
- **Additional Liferay Node**: For clustering scenarios

To enable these services, uncomment the relevant sections in the docker-compose.yaml file.

### Network

All services are connected through the `lr-network` network for internal communication.

## Notes

- This setup is for local development only. For production, review security, performance, and clustering settings.
- For clustering or search capabilities, uncomment and configure the `lr-elasticsearch` and additional Liferay nodes as needed.

## References

- [Liferay DXP 7.4 Documentation](https://learn.liferay.com/dxp/latest/en/)
- [Liferay Docker Images](https://hub.docker.com/r/liferay/portal)
- [Liferay Workspace & OSGi Best Practices](https://learn.liferay.com/dxp/latest/en/building-applications/core-frameworks/osgi/osgi-best-practices.html) 
