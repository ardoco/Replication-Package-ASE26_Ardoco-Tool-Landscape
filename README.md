# Replication Package for "The ARDoCo Tool Landscape: REST API, TraceView, and TraceViz for Architecture Traceability"

by Jan Keim, Dominik Fuchß, Sophie Corallo, Tobias Hey, Julian Winter, and Kevin Feichtinger

This is the replication package for our ASE 2026 paper on the ARDoCo tool landscape.
It bundles the three presented tools together with the underlying ARDoCo framework source and the code model extractor.

**Screencast**: https://youtu.be/IOTEPZQ3tVs

## Overview

The names in the paper describe two levels: the **tools** (REST API, TraceView, TraceViz) are how you access the functionality, while the **approaches** are the underlying traceability link recovery (TLR) techniques they make accessible. The table below maps each approach to the artifacts it links and to the tools that expose it.

| Approach                  | Artifacts linked              | REST API | TraceView | TraceViz |
| ------------------------- | ----------------------------- | :------: | :-------: | :------: |
| **SWATTR**                | SAD ↔ SAM                     |    ✓     |     ✓     |    —     |
| **ArCoTL**                | SAM ↔ Code                    |    ✓     |     ✓     |    —     |
| **ArDoCode**              | SAD ↔ Code                    |    ✓     |     ✓     |    ✓     |
| **TransArC**              | SAD ↔ SAM ↔ Code (transitive) |    ✓     |     ✓     |    ✓     |
| **LiSSA** (LLM/RAG-based) | SAD ↔ Code                    |    —     |     —     |    ✓     |

*SAD = software architecture documentation, SAM = software architecture model, Code = code model (extracted from the source tree; see [Preparing a Code Model](#preparing-a-code-model)).*

## Structure

* [`rest/`](rest/) — ARDoCo REST API: Spring Boot service exposing all TLR pipelines (SWATTR, ArCoTL, ArDoCode, TransArC) via HTTP
* [`traceview-v2/`](traceview-v2/) — TraceView: browser-based UI optimized for SAD-SAM-Code TLR (publicly available at [tv.ardoco.de](https://tv.ardoco.de))
* [`traceviz/`](traceviz/) — TraceViz: VS Code extension for in-IDE SAD-Code trace link visualization
* [`code-model-extractor-cli/`](code-model-extractor-cli/) — CLI tool for generating the code model required by SAM-Code and SAD-Code pipelines
* [`ardoco/`](ardoco/) — ARDoCo v2.0.1 framework source (reference; not built separately)
* [`examples/`](examples/) — TeaStore example inputs from the ARDoCo benchmark: `teastore.txt` (SAD), `teastore.uml` (SAM, UML format), `code-model.acm` (pre-generated code model)

Each subdirectory contains a README with component-specific documentation.

## Public Deployments

* REST API: https://rest.ardoco.de
* TraceView: https://tv.ardoco.de
* TraceViz: https://github.com/ardoco/traceviz

---

## Part 1: Getting Started

**Estimated time: 5 min for the first build (Maven and npm dependencies are downloaded from the internet); subsequent runs start in under a minute from cached layers.**

### Installation

Requires Docker with Compose. Build and start the REST API and TraceView:

```bash
docker compose up --build -d
```

### Smoke Test

Verify both services are up:

```bash
curl http://localhost:8080/actuator/health   # expects {"status":"UP",...}
curl -o /dev/null -s -w "%{http_code}" http://localhost:3000  # expects 200
```

Open http://localhost:3000 in a browser to confirm TraceView loads.

---

## Part 2: Step-by-Step Instructions

### Claims Supported by This Artifact

The paper presents three tools; this artifact allows verification of all three:

1. **ARDoCo REST API** — exposes four TLR pipelines (SWATTR, ArCoTL, ArDoCode, TransArC) via HTTP with asynchronous execution and Redis-backed caching. The Swagger UI at http://localhost:8080/swagger-ui/index.html documents all endpoints.

2. **TraceView** — browser-based frontend for SAD-SAM-Code TLR. Accessible at http://localhost:3000 (or https://tv.ardoco.de for the public deployment).

3. **TraceViz** — VS Code extension for in-IDE SAD-Code TLR visualization. See [`traceviz/README.md`](traceviz/README.md) for development setup.

This artifact demonstrates tool availability and functionality.
The benchmark reproducibility has been verified in the original papers and their replication packages.

### Verifying the REST API

The `examples/` directory contains ready-to-use TeaStore inputs from the ARDoCo benchmark.
Run the following from the repository root to execute a SAD-SAM pipeline:

```bash
curl -X POST http://localhost:8080/api/swattr/start-and-wait \
  -F "projectName=teastore" \
  -F "inputText=@examples/teastore.txt" \
  -F "inputArchitectureModel=@examples/teastore.uml" \
  -F "inputArchitectureModelFormat=UML"
```

The response contains recovered SAD-SAM trace links and detected inconsistencies in JSON format.
All endpoints can also be explored interactively via the Swagger UI at http://localhost:8080/swagger-ui/index.html.

### Verifying TraceView

1. Open http://localhost:3000 in a browser.
2. Click the API address field in the navigation bar and set it to `http://rest:8080`.
3. Start a new project via the wizard, uploading the files from `examples/`:
   - SAD: `teastore.txt`
   - SAM: `teastore.uml` (UML)
   - Code model: `code-model.acm`
4. Select a pipeline (e.g., TransArC for full SAD-SAM-Code TLR) and submit.
5. After processing, the multi-panel result view shows the artifacts and recovered trace links side by side.

### Verifying TraceViz

See [`traceviz/README.md`](traceviz/README.md) for how to launch the extension in a VS Code Extension Development Host.
To use it against the local REST API, configure the endpoint to `http://localhost:8080` in the TraceViz settings.

### Preparing a Code Model

SAM-Code and SAD-Code pipelines require a code model generated from the Java source tree of the system under analysis:

```bash
cd code-model-extractor-cli
mvn package
java -jar target/code-model-extractor.jar /path/to/java/src model.json
```

See [`code-model-extractor-cli/README.md`](code-model-extractor-cli/README.md) for details.

## License

MIT — see [`LICENSE`](LICENSE).
