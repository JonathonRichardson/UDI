# The UDI Specification

## Status of this document

This document specifies the architecture of UDI applications and the UDI platform. Distribution of this document is unlimited.

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [RFC 2119](https://tools.ietf.org/html/rfc2119).

## Platform vs Application

**Applications** are comprised of a set of containerized services described by a compose specification.

Applications are deployed to **platforms**.  Platforms provide **components** which expose the functionality of the platform.  The default implementation is based on Docker Desktop, but other platforms should be able to be developed from Azure, AWS, GCP, or any other service that can generally deploy containers, especially where Kubernetes is supported.

Platform components are as follows:

|Component|Description|
|---------|-----------|
|Gateway  |Provides ingestion routing into the **application**|


## Project Repository Structure

The project should have a single repository of entry (e.g. a "monorepo").  This should be able to be run independently of any other source code.

If there is a need to break up a project, the integrity of the main repo should be maintained by sub-repos being published as docker images, which can be referenced in the main repository.  However, it is generally not recommended to break up a project repository unless there is a significant problem.

## Build Artifacts

Project builds produce one of three types of artifacts:

* Docker Image(s)
* Static Site Archive(s)
* Metadata Config
  * This is in the form of a compose or kubernetes spec.

## Directory Structure

```
project_dir/
|
├─ .env
├─ README.md
|
├─ .build-cache/
|  └─ {lang}/
|
├─ lib/
|  └─ {lang}/
|
├─ specs/
|
└─ services/
   └─ {service_name}
       └─ docker-compose.yml
```

The root `.env` file provides some global environment variables that can be used by other applications:

|Environment Variable|Usage/Meaning|
|--------------------|-------------|
|`PROJECT_DOMAIN`    |Unique domain for the project.  This can be overidden to run multiple copies of the app in parallel on the same machine.|

The `.build-cache/` directory is for intermediate compilation output, and should be organized by language/sdk.  This is language specific.  For example, for C#, this is where the `bin/` and `obj/` folders are kept.  This is only necessary for builds that produce intermediate files, and not general compilation results, to avoid conflicts between builds inside containers and builds on the host.  Nothing in this directory should be source controlled, other than `.gitkeep` and `.gitignore` files.

The `lib/` directory is for shared code that multiple services need to access, and should be organized by language/runtime.  Code that doesn't need to be shared should be under `services/`.

`services/` is where each component of the application is stored.  Each directory should contain a `docker-compose.yml` file to defined the images for that service.  Services should only communicate with each other via TCP connections via their service names.

Whenever possible, services should be self contained within their service file.  Any exceptions should be documented and reasoned.  Services should only bind mount volumes from within their service directory, `lib/`, and `specs/`.  They should not mount files from another service directory.