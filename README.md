# Replication Package for "The ARDoCo Tool Landscape: REST API, TraceView, and TraceViz for Architecture Traceability"

by Jan Keim, Dominik Fuchß, Sophie Corallo, Tobias Hey, Julian Winter, and Kevin Feichtinger

This is the replication package for our ASE 2026 paper on the ARDoCo tool landscape.
It bundles the three presented tools together with the underlying ARDoCo framework source and the code model extractor.

**Screencast**: https://youtu.be/IOTEPZQ3tVs

## Structure

* [`rest/`](rest/) — ARDoCo REST API: Spring Boot service exposing all TLR pipelines (SWATTR, ArCoTL, ArDoCode, TransArC) via HTTP
* [`traceview-v2/`](traceview-v2/) — TraceView: browser-based UI optimized for SAD-SAM-Code TLR (publicly available at [tv.ardoco.de](https://tv.ardoco.de))
* [`traceviz/`](traceviz/) — TraceViz: VS Code extension for in-IDE SAD-Code trace link visualization
* [`code-model-extractor-cli/`](code-model-extractor-cli/) — CLI tool for generating the code model required by SAM-Code and SAD-Code pipelines
* [`ardoco/`](ardoco/) — ARDoCo v2.0.1 framework source (reference; not built separately)

Each subdirectory contains a README with component-specific documentation.

## Public Deployments

* REST API: https://rest.ardoco.de
* TraceView: https://tv.ardoco.de
* TraceViz: https://github.com/ardoco/traceviz

## Quick Start (Docker Compose)

Builds and starts the REST API and TraceView, both bound to `localhost`:

```bash
docker compose up --build -d
```

* TraceView: http://localhost:3000
* REST API (Swagger UI): http://localhost:8080/swagger-ui/index.html

**TraceView:** defaults to `rest.ardoco.de`; set the API address to `http://rest:8080` via the navigation bar to use the local instance (the Next.js proxy runs inside the Docker network).

**TraceViz:** runs in VS Code outside Docker; configure its endpoint to `http://localhost:8080`. See [`traceviz/README.md`](traceviz/README.md).

## Preparing a Code Model

SAM-Code and SAD-Code pipelines require a code model generated from the Java source tree of the system under analysis:

```bash
cd code-model-extractor-cli
mvn package
java -jar target/code-model-extractor.jar /path/to/java/src model.json
```

See [`code-model-extractor-cli/README.md`](code-model-extractor-cli/README.md) for details.

## License

MIT — see [`LICENSE`](LICENSE).
