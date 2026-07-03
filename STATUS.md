# Artifact Status

We apply for the following ACM artifact evaluation badges:

## Available

The artifact is permanently archived on Zenodo with a DOI

## Functional

All three tools presented in the paper are included in this repository and are exercisable:

* The **ARDoCo REST API** can be built and started via Docker Compose; all four TLR pipelines are accessible through the Swagger UI.
* **TraceView** launches as a Next.js application via Docker Compose and provides the guided wizard and multi-panel result view described in the paper.
* **TraceViz** can be built from source and run in a VS Code Extension Development Host.

The `README.md` provides a smoke test and step-by-step instructions for verifying each component.

## Reusable

The artifact exceeds functional standards:

* All components are released under permissive open-source licenses (MIT).
* Docker images are published to the GitHub Container Registry (`ghcr.io/ardoco/rest`, `ghcr.io/ardoco/traceview`).
* The REST API is publicly deployed at https://rest.ardoco.de and can be used independently of this package.
* TraceView is publicly deployed at https://tv.ardoco.de.
* The components are designed for reuse: the REST API can be integrated into any HTTP-capable tool; TraceViz supports CSV import for any third-party TLR tool.
