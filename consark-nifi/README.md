# Consark NiFi

## Overview
Consark NiFi is a custom implementation of Apache NiFi designed to streamline data integration and workflow automation for business intelligence applications. This project extends Apache NiFi's capabilities to include custom processors, templates, and configurations specifically optimized for ETL processes in enterprise environments.

## Table of Contents
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)
- [Project Structure](#project-structure)
- [Custom Processors](#custom-processors)
- [Templates](#templates)
- [Monitoring](#monitoring)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [License](#license)
- [Contact](#contact)

## Prerequisites
- Apache NiFi version 1.15.0 or higher
- Java 11 or higher
- Maven 3.6.3 or higher
- Docker 20.10.x (for containerized deployment)
- 4GB RAM minimum (8GB recommended)

## Installation

### Local Installation
1. Clone the repository:
    ```bash
    git clone https://github.com/consark/consark-nifi.git
    cd consark-nifi
    ```

2. Build the project:
    ```bash
    mvn clean install
    ```

3. Deploy custom NAR files to your NiFi installation:
    ```bash
    cp nifi-consark-nar/target/nifi-consark-nar-*.nar $NIFI_HOME/lib/
    ```

4. Restart NiFi:
    ```bash
    $NIFI_HOME/bin/nifi.sh restart
    ```

### Docker Installation
1. Build the Docker image:
    ```bash
    docker build -t consark/nifi:latest .
    ```

2. Run the container:
    ```bash
    docker run -d -p 8080:8080 -p 8443:8443 --name consark-nifi consark/nifi:latest
    ```

## Configuration

### Basic Configuration
1. Navigate to the `conf` directory to modify configuration files:
    - `nifi.properties`: Core NiFi settings
    - `consark-config.properties`: Consark-specific settings
    - `authorizers.xml`: Security settings

2. Default ports:
    - Web UI: 8080 (HTTP) / 8443 (HTTPS)
    - Service ports: Configured in `nifi.properties`

### Security Configuration
1. Enable SSL by modifying `nifi.properties`:
    ```properties
    nifi.security.keystoreType=JKS
    nifi.security.keystorePath=./conf/keystore.jks
    nifi.security.keystorePasswd=password
    nifi.security.keyPasswd=password
    nifi.security.truststoreType=JKS
    nifi.security.truststorePath=./conf/truststore.jks
    nifi.security.truststorePasswd=password
    ```

2. Set up authentication in `authorizers.xml`

## Usage

### Starting Consark NiFi
```bash
./bin/consark-nifi.sh start
```

### Accessing the Web UI
Open your browser and navigate to:
- HTTP: `http://localhost:8080/nifi`
- HTTPS: `https://localhost:8443/nifi`

### Importing Templates
1. In the NiFi UI, click on the Templates icon in the top right
2. Select "Upload Template" and choose one of the templates from the `templates` directory
3. Drag the template icon onto the canvas and select the uploaded template

## Project Structure
```
consark-nifi/
├── nifi-consark-processors/      # Custom processor implementations
├── nifi-consark-nar/             # NAR packaging for custom processors
├── templates/                    # Pre-configured flow templates
├── docker/                       # Docker configuration files
├── conf/                         # Configuration files
├── scripts/                      # Utility scripts
└── docs/                         # Documentation
```

## Custom Processors
Consark NiFi includes the following custom processors:

| Processor | Description |
|-----------|-------------|
| ConsarkETLProcessor | Handles ETL operations with built-in data transformation |
| ConsarkDataEnricher | Enriches data with additional context from external sources |
| ConsarkValidationProcessor | Performs data validation against predefined schemas |
| ConsarkNotificationProcessor | Sends notifications through various channels |

For detailed documentation on each processor, see the `docs/processors/` directory.

## Templates
Ready-to-use templates are available in the `templates/` directory:

- `data-warehouse-integration.xml` - Template for connecting to data warehouses
- `real-time-analytics.xml` - Flow for real-time data processing
- `error-handling.xml` - Comprehensive error handling pattern
- `data-quality-pipeline.xml` - Data quality assessment workflow

## Monitoring

### Built-in Monitoring
Consark NiFi extends the standard NiFi monitoring with:
- Enhanced metrics collection
- Custom dashboards for business KPIs
- Alerting mechanisms for critical failures

### Integration with Monitoring Systems
- Prometheus endpoint: `/nifi-api/consark/metrics`
- Grafana dashboards available in `monitoring/dashboards/`
- JMX beans for Java-based monitoring tools

## Troubleshooting

### Common Issues
- **Processor fails to start**: Check the logs at `logs/nifi-app.log`
- **Performance issues**: Review `Performance Tuning` in the documentation
- **Connection errors**: Verify network settings and credentials

### Logs
- Application logs: `logs/nifi-app.log`
- Bootstrap logs: `logs/nifi-bootstrap.log`
- Custom processor logs: `logs/consark-processors.log`

## Contributing
1. Fork the repository
2. Create a feature branch: `git checkout -b feature/new-feature`
3. Commit changes: `git commit -am 'Add new feature'`
4. Push to branch: `git push origin feature/new-feature`
5. Submit a pull request

See [CONTRIBUTING.md](CONTRIBUTING.md) for detailed guidelines.

## License
This project is licensed under the Apache License 2.0 - see the [LICENSE](LICENSE) file for details.

## Contact
- Project Maintainer: [Akshay V](mailto:vakshay@consark.ai)
- Issues: [GitHub Issues](https://github.com/consark/consark-nifi/issues)
- Slack: [#consark-nifi](https://consark.slack.com/channels/consark-nifi)
