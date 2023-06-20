# GitHub repository standards
This repository contains the reference on how GitHub repositories should be organized from the development and infrastructure standpoint. It consists of GitHub actions for image building, security, and other useful tests.

## Regular Git Branches
These branches will be available in your repository on permanent bases. Their naming convention is simple and straightforward.:
- **main** - latest production-ready release code
- **stage** *(optional)* - latest pre-production release code
- **test** - latest test environment release code
- **dev** - ongoing development goes here

## Temporary Git Branches
As the name indicates, these are the branches that can be created and deleted when needed. They can be as follows:

- Bug Fix
- Hot Fix
- Feature Branches
- Experimental Branches
- WIP branches

## Tags
Tags should start with **v** and be [semver](https://semver.org/) compatible. Example: `v1.2.3`. Tags are being used for building and tagging production-ready container images.
