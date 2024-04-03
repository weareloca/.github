# GitHub repository standards
This repository contains the reference on how GitHub repositories should be organized from the development and infrastructure standpoint. It consists of GitHub actions for image building, security, and other useful actions.

## Git Branching Model
SnapAds development flow incorporates the git branching model called `Git Flow`, introduced by Vincent Driessen in 2010. Some can find useful this tool at https://github.com/nvie/gitflow. Based on the recommendations from Vincent Driessen, we use the next branches permanently. Their naming convention is simple and straightforward.:

## Allowed Git Branch Names
As the name indicates, these are the branches that can be created and deleted when needed. They can be as follows:

- **Main** contains the latest code, that has been used for creating the recent release tag (`latest` tag)
- **Develop** - ongoing development goes here (develop)
- **Bugfix** (bugfix/XXX-42-explanatory_name)
- **Hotfix** (hotfix/XXX-42-explanatory_name)
- **Feature** (feature/XXX-42-explanatory_name)
- **Support** (support/XXX-42-explanatory_name)
- **Test** (test/XXX-42-explanatory_name)
- **Release** (release/v1.2.3)

## Tags
Tags should be [semver](https://semver.org/) compatible. Example: `v1.2.3`, `v1.0.0-alpha` or `v2.0.0-rc.2`. Tags are being used for building and tagging production-ready container images.

## Commit message
Please, follow https://www.conventionalcommits.org/ when you create a commit message as often as possible.

### Feature Development Phase
- **Feature Branch Creation**: Developers create a feature branch from the release branch.
- **Feature Implementation**: Developers work on implementing the feature, committing changes to the feature branch.
- **Code Review**: Code review occurs when developers create a pull request to merge changes from the feature branch into the release branch.

### Stabilization Phase
- **Merge to Release Branch**: Once features are completed and deemed stable, they're merged into the release branch.
- **Testing**: The release branch is continuously deployed to the test environment for comprehensive testing as it gets updated. Images are built from the release branch updates, and the test environment is updated accordingly.

### Release Preparation Phase
- **Pull Request to Main Branch**: Once all features are tested on the test environment and deemed ready, a pull request is created from the release branch into the main branch.
- **Tagging**: A tag is created with the same version as the release branch.

### Deployment Phase
- **Image Building**: Images are built from the tagged version.
- **Deployment to Production**: The built images are deployed to the production environment.

