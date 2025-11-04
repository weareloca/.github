# GitHub repository standards
This repository contains the reference on how GitHub repositories should be organized from the development and infrastructure standpoint. It consists of GitHub actions for image building, security, and other useful actions.

## Git Branching Model
Loca development flow incorporates the git branching model called `Git Flow`, introduced by Vincent Driessen in 2010. Some can find useful this tool at https://github.com/nvie/gitflow. Based on the recommendations from Vincent Driessen, we use the next branches permanently. Their naming convention is simple and straightforward.:

## Allowed Git Branch Names
As the name indicates, these are the branches that can be created and deleted when needed. They can be as follows:

- **Main** contains the latest code, that has been used for creating the recent release tag (`latest` tag)
- **Develop** - ongoing development goes here (develop)
- **Release** (release/v1.2.3)
- **Bugfix** (bugfix/XXX-42-explanatory_name)
- **Hotfix** (hotfix/XXX-42-explanatory_name)
- **Feature** (feature/XXX-42-explanatory_name)
- **Support** (support/XXX-42-explanatory_name)
- **Test** (test/XXX-42-explanatory_name)

## Tags
Tags should be [semver](https://semver.org/) compatible. Example: `v1.2.3`, `v1.0.0-alpha` or `v2.0.0-rc.2`. Tags are being used for building and tagging production-ready container images.

## Commit message
Please, follow https://www.conventionalcommits.org/ when you create a commit message as often as possible.

## Changelog
We follow the [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) format for maintaining changelogs. Each project should maintain a `CHANGELOG.md` file that documents all notable changes in a clear and organized manner. The changelog should:

- List changes in reverse chronological order (newest first)
- Group changes by version
- Use the following categories: Added, Changed, Deprecated, Removed, Fixed, Security
- Include the release date for each version
- Follow [semantic versioning](https://semver.org/)

For more details on changelog best practices, please refer to https://keepachangelog.com/en/1.1.0/

## Workflow Documentation

For details on our development and deployment workflows, please see the following documents:

- [Regular Feature Workflow](../flows/regular_feature.md) - Steps for developing and integrating new features within the regular release cycle.
- [Hotfix/Bugfix Workflow Without Testing](../flows/hotfix_bugfix_without_testing.md) - Steps for urgent fixes that need immediate deployment without prior testing.

## Improvement Recommendations

Looking to improve your development workflow? Check out these resources:

- **[Startup Improvements Guide](../STARTUP_IMPROVEMENTS.md)** - Comprehensive recommendations for balancing code quality with development velocity. Prioritized by impact vs. effort, specifically tailored for small startups.
- **[Quick Start Guide](../QUICK_START.md)** - Step-by-step instructions to implement recommended improvements, with commands and examples for our tech stack (Laravel, NestJS, Next.js, Terraform).

## CI/CD Templates

Ready-to-use GitHub Actions workflow templates for our tech stack:

- **[Node.js CI](../workflow-templates/node-ci.yml)** - For NestJS and Next.js projects
- **[Laravel CI](../workflow-templates/laravel-ci.yml)** - For Laravel PHP applications
- **[Terraform CI](../workflow-templates/terraform-ci.yml)** - For infrastructure as code
- **[Lambda/SAM CI](../workflow-templates/lambda-sam-ci.yml)** - For AWS Lambda functions

See the [workflow templates README](../workflow-templates/README.md) for detailed setup instructions.

## Repository Templates

Use these templates to maintain consistency across projects:

- **[Pull Request Template](../PULL_REQUEST_TEMPLATE.md)** - Ensures PRs include necessary information
- **[Issue Templates](../ISSUE_TEMPLATE/)** - Bug reports, feature requests, and technical debt tracking
- **[CODEOWNERS Example](../CODEOWNERS.example)** - Automatic reviewer assignment
- **[Dependabot Config](../dependabot.yml.example)** - Automated dependency updates
- **[Security Policy](../SECURITY.md.example)** - Guidelines for reporting security vulnerabilities

Please follow these guidelines to maintain consistency and quality in project contributions.
