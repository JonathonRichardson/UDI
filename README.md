# UDI (Universal Developer Interface)

The aim of UDI is to provide a standard specification for the architecture of applications to make the lives of developers and devops engineers easier.  The ultimate goal is that a developer should be able to onboard onto any UDI-compliant application with no extra setup time beyond installation of underlying tooling.  As for devops engineers, the goal is that it should be easy to look at any UDI app and create deployment scripts to advance the application down the devops pipeline to production.

Tooling can be developed to facilitate compliance with the specification and to provide a compliant platform.

Although UDI is primarily focused on SaaS applications deployed to the cloud, the tooling and tenants are also applicable to other styles of application.

## Version History


*The current spec is still in draft.  No releases yet.*

## Implementations

*There are no implementations yet*.

## Tenets

### Versioning and Extensions

The UDI spec should be versioned (using semver) so that users can positively identify which features the platform provides which they can rely on, and updates can be made to the underlying specification.

### Congruence Between Development and Deployment Environments

The closer development environments can be to their deployed counterparts, the fewer problems arise when deploying applications.

This also means that the application should be able to be deployed to different deployment targets (AWS, Azure, GCP, etc.) without issue.

For these reasons, components and interactions should be rely on services and components that are generally available in all languages and platforms (e.g. file IO, TCP, DNS, containers, etc).

### Instant Setup Development Environments

Even in 2022, it is not uncommon for development environments to take hours or days to set up.

The goal of this project is to allow development environments to be as easy to set up as:

* `git clone ....`
* `docker udi-compose up`

All tooling, development tools, etc should come bundled in the main repo.

### No Conflict Between Development Environments

Developers should be able to have multiple UDI applications on their dev machine without them conflicting with each other.

In addition, as much as possible, UDI tooling should attempt to incur as few global dependencies, so that it is quick to install, has a low chance of failing to install, and doesn't needlessly conflict with non-UDI applications.