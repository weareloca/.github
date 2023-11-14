# GitHub repository standards
This repository contains the reference on how GitHub repositories should be organized from the development and infrastructure standpoint. It consists of GitHub actions for image building, security, and other useful actions.

## Git Branching Model
SnapAds development flow incorporates the git branching model called `Git Flow`, introduced by Vincent Driessen in 2010. Some can find useful this tool at https://github.com/nvie/gitflow. Based on the recommendations from Vincent Driessen, we use the next branches permanently. Their naming convention is simple and straightforward.:

## Allowed Git Branch Names
As the name indicates, these are the branches that can be created and deleted when needed. They can be as follows:

- **Main** contains the latest code, that has been used for creating the recent release tag (main)
- **Develop** - ongoing development goes here (develop)
- **Bugfix** (bugfix/XXX-42-explanatory_name)
- **Hotfix** (hotfix/XXX-42-explanatory_name)
- **Feature** (feature/XXX-42-explanatory_name)
- **Release** (release/v1.2.3)

## Tags
Tags should be [semver](https://semver.org/) compatible. Example: `1.2.3`, `1.0.0-alpha` or `2.0.0-rc.2`. Tags are being used for building and tagging production-ready container images.

## Commit message
Please, follow https://www.conventionalcommits.org/ when you create a commit message as often as possible.
