# GitHub repository standards
This repository contains the reference on how GitHub repositories should be organized from the development and infrastructure standpoint. It consists of GitHub actions for image building, security, and other useful tests.

## Branches and tags
Common branches that should be implemented:
- **main** - latest production-ready release code
- **stage** *(optional)* - latest pre-production release code
- **test** - latest test environment release code
- **dev** - ongoing development goes here
- feature branches (for instance XXX-123). No strict rules here, agree on conventions with your teammates.

Tags should start with **v** and be [semver](https://semver.org/) compatible. Example: `v1.2.3`.
Tags are being used for building and tagging production-ready container images.
