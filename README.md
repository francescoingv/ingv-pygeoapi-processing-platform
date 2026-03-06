
# INGV pygeoapi processing platform

This repository describes the **INGV pygeoapi processing platform**, a
software architecture designed to expose and execute scientific
processing services through APIs compliant with the **OGC API - Processes**
standard.

The platform integrates **pygeoapi**, custom process plugins, and an
external execution service that runs scientific codes in an isolated
environment.

---

## Overview

The platform enables the publication of scientific processing services
through standard web APIs.

In this architecture:

- **pygeoapi** exposes processes through APIs compliant with the
  **OGC API - Processes** specification
- **pygeoapi plugins** implement the processing endpoints
- an **external execution service** runs the scientific codes
- scientific codes are executed independently from the API layer

This separation allows:

- independent execution environments
- flexible deployment of processing codes
- better isolation of software dependencies

---

## Platform architecture

The platform consists of four main layers.

### 1. pygeoapi

The **pygeoapi** framework exposes processes through APIs compliant with
the **OGC API - Processes** standard.

https://pygeoapi.io/

---

### 2. pygeoapi process plugins

The repository

https://github.com/francescoingv/ingv-pygeoapi-process-plugins

contains the plugins implementing pygeoapi processes.

These plugins receive execution requests through the OGC API interface
and forward them to an external execution service.

The service is responsible for:

- managing input and output parameters
- invoking the execution service
- collecting execution results from execution service
- storing job information and results in a PostgreSQL database

---

### 3. Execution service

The execution service is implemented in the repository:

https://github.com/francescoingv/generic-processor-provider

This service receives HTTP requests from the plugins and invokes the
configured scientific codes through command-line execution.

The service is responsible for:

- invoking the processing code
- managing execution parameters
- collecting execution results from processing code
- returning execution results

---

### 4. Scientific processing codes

The scientific processing codes are **not part of this platform
repository**.

They are invoked by the execution service through the `command_line`
parameter defined in the service configuration.

This design allows:

- independent development of scientific codes
- flexible deployment of different processing applications
- reuse of the same execution infrastructure for multiple codes

---

## Logical workflow

Client
  │
  ▼
pygeoapi
  │
  ▼
pygeoapi plugins
  │
  ▼
generic-processor-provider
  │
  ▼
scientific processing code

Workflow description:

1. A client sends a processing request to **pygeoapi**.
2. **pygeoapi** forwards the request to the appropriate plugin.
3. The **pygeoapi plugin** sends the execution request to the external
   execution service.
4. The **execution service** invokes the configured scientific code.
5. The result is returned to the plugin.
6. **pygeoapi** exposes the result through the OGC API interface.

---

## Platform repositories

The platform is composed of the following software repositories:

- https://github.com/francescoingv/ingv-pygeoapi-process-plugins
- https://github.com/francescoingv/generic-processor-provider

---

## Citation

If you use this platform in scientific work, please cite it as:

Martinelli, F. (2026).
INGV pygeoapi processing platform

The DOI will be added after Zenodo publication.

---

## License

This project is distributed under the MIT License.

See the LICENSE file for details.

---

## Author

Francesco Martinelli
Istituto Nazionale di Geofisica e Vulcanologia (INGV)
Pisa, Italy
