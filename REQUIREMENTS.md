# Requirements

## Hardware

No special hardware required. The Docker Compose setup (REST API + Redis + TraceView) runs comfortably on any modern laptop or desktop.

Minimum recommended: 8 GB RAM available to Docker, 5 GB free disk space.

## Software

### For the Docker Compose Deployment (REST API + TraceView)

* Docker Engine ≥ 24.0 with Compose V2 (`docker compose`)
* Internet access during the first build (Maven and npm dependencies are downloaded at build time; subsequent builds use the Docker layer cache)
* Any modern browser (Chrome, Firefox, Edge, Safari) without private/ incognito mode.

### For TraceViz (VS Code Extension)

* Visual Studio Code ≥ 1.100.0
* Node.js ≥ 20 and npm (for building from source)

### For the Code Model Extractor

* Java JDK ≥ 21
* Apache Maven ≥ 3.9

## Architecture

The Docker images run on both `linux/amd64` and `linux/arm64` (Apple Silicon).
