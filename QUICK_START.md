# Quick Start Guide for Implementing Improvements

This guide provides step-by-step instructions to implement the recommended improvements from [STARTUP_IMPROVEMENTS.md](STARTUP_IMPROVEMENTS.md).

## Week 1: Foundation Setup (4-6 hours)

### Day 1-2: Templates and Basic Configuration (2 hours)

1. **Copy templates to your repositories**
   ```bash
   # From this repository to your project repository
   cp .github/PULL_REQUEST_TEMPLATE.md /path/to/your-repo/.github/
   cp -r .github/ISSUE_TEMPLATE /path/to/your-repo/.github/
   ```

2. **Setup CODEOWNERS**
   ```bash
   # Copy and customize the example
   cp .github/CODEOWNERS.example /path/to/your-repo/.github/CODEOWNERS
   # Edit to match your team structure
   ```

3. **Enable Dependabot**
   ```bash
   # Copy and customize
   cp .github/dependabot.yml.example /path/to/your-repo/.github/dependabot.yml
   # Edit to match your tech stack
   ```

### Day 3-4: Code Formatting (2-4 hours)

#### For NestJS/Next.js projects (Node.js/TypeScript):

```bash
# Install Prettier
npm install --save-dev prettier

# Create config
cat > .prettierrc << EOF
{
  "semi": true,
  "trailingComma": "es5",
  "singleQuote": true,
  "printWidth": 100,
  "tabWidth": 2
}
EOF

# Add to package.json scripts
# "format": "prettier --write ."
# "format:check": "prettier --check ."

# Format existing code
npm run format

# Install husky for pre-commit hooks
npm install --save-dev husky lint-staged
npx husky init
cat > .husky/pre-commit << 'EOF'
npx lint-staged
EOF

# Configure lint-staged in package.json
# "lint-staged": {
#   "*.{js,jsx,ts,tsx}": ["prettier --write", "eslint --fix"],
#   "*.{json,md}": ["prettier --write"]
# }
```

#### For Laravel projects (PHP):

```bash
# Laravel Pint comes with Laravel 9+, just configure it
cat > pint.json << EOF
{
    "preset": "laravel",
    "rules": {
        "simplified_null_return": true,
        "braces": false,
        "new_with_braces": true
    }
}
EOF

# Run Pint to format code
./vendor/bin/pint

# Add to composer.json scripts
# "pint": "pint",
# "pint-test": "pint --test"

# Setup Git hooks with GrumPHP (optional)
composer require --dev phpro/grumphp
cat > grumphp.yml << EOF
grumphp:
    tasks:
        phplint: ~
        phpstan:
            level: 5
        phpunit: ~
EOF
```

#### For Terraform projects:

```bash
# Format all Terraform files (built-in)
terraform fmt -recursive

# Setup pre-commit hook
cat > .git/hooks/pre-commit << 'EOF'
#!/bin/sh
terraform fmt -check -recursive
if [ $? -ne 0 ]; then
  echo "Terraform files are not formatted. Run: terraform fmt -recursive"
  exit 1
fi
EOF
chmod +x .git/hooks/pre-commit

# Or use pre-commit framework
pip install pre-commit
cat > .pre-commit-config.yaml << EOF
repos:
  - repo: https://github.com/antonbabenko/pre-commit-terraform
    rev: v1.88.0
    hooks:
      - id: terraform_fmt
      - id: terraform_validate
      - id: terraform_tflint
EOF

pre-commit install
```

## Week 2: Basic CI/CD (4-8 hours)

### Setup GitHub Actions

1. **Choose the appropriate workflow template**
   - `workflow-templates/node-ci.yml` for NestJS/Next.js projects
   - `workflow-templates/laravel-ci.yml` for Laravel projects
   - `workflow-templates/terraform-ci.yml` for Terraform infrastructure
   - `workflow-templates/lambda-sam-ci.yml` for AWS Lambda/SAM projects

2. **Copy to your repository**
   ```bash
   mkdir -p .github/workflows
   # Example for NestJS/Next.js
   cp workflow-templates/node-ci.yml /path/to/your-repo/.github/workflows/ci.yml
   # Or for Laravel
   cp workflow-templates/laravel-ci.yml /path/to/your-repo/.github/workflows/ci.yml
   ```

3. **Customize for your project**
   - Update PHP/Node.js versions if needed
   - Adjust database services (MySQL, PostgreSQL, Redis)
   - Configure test commands and coverage thresholds
   - Add environment variables

4. **Test the workflow**
   - Create a test branch
   - Make a small change
   - Push and create a PR
   - Verify CI runs successfully

### Setup Branch Protection

1. Go to GitHub repo → Settings → Branches
2. Add rule for `main` branch:
   - ✅ Require a pull request before merging
   - ✅ Require approvals (1 minimum)
   - ✅ Require status checks to pass
   - ✅ Select your CI workflow
   - ✅ Require branches to be up to date

## Week 3-4: Code Quality Tools (4-6 hours)

### Setup Linting

#### For NestJS/Next.js (Node.js/TypeScript):

```bash
# Install ESLint and TypeScript support
npm install --save-dev eslint @typescript-eslint/parser @typescript-eslint/eslint-plugin

# For Next.js
npm install --save-dev eslint-config-next

# For NestJS
npm install --save-dev @nestjs/eslint-plugin

# Create .eslintrc.json
cat > .eslintrc.json << EOF
{
  "parser": "@typescript-eslint/parser",
  "parserOptions": {
    "project": "tsconfig.json",
    "sourceType": "module"
  },
  "plugins": ["@typescript-eslint"],
  "extends": [
    "eslint:recommended",
    "plugin:@typescript-eslint/recommended"
  ],
  "rules": {
    "@typescript-eslint/no-unused-vars": "error",
    "@typescript-eslint/explicit-function-return-type": "off"
  }
}
EOF

# Add to package.json scripts
# "lint": "eslint \"{src,apps,libs,test}/**/*.ts\""
# "lint:fix": "eslint \"{src,apps,libs,test}/**/*.ts\" --fix"
```

#### For Laravel (PHP):

```bash
# Install PHPStan for static analysis
composer require --dev phpstan/phpstan

# Create phpstan.neon
cat > phpstan.neon << EOF
parameters:
    level: 5
    paths:
        - app
        - routes
    excludePaths:
        - app/Console/Kernel.php
    checkMissingIterableValueType: false
EOF

# Run PHPStan
./vendor/bin/phpstan analyse

# Add to composer.json scripts
# "phpstan": "phpstan analyse",
# "test": "pest",
# "pint": "pint",
# "check": ["@phpstan", "@pint-test", "@test"]
```

#### For Terraform:

```bash
# Install tflint
curl -s https://raw.githubusercontent.com/terraform-linters/tflint/master/install_linux.sh | bash

# Create .tflint.hcl
cat > .tflint.hcl << EOF
plugin "terraform" {
  enabled = true
  preset  = "recommended"
}

plugin "aws" {
  enabled = true
  version = "0.29.0"
  source  = "github.com/terraform-linters/tflint-ruleset-aws"
}
EOF

# Initialize tflint
tflint --init

# Run tflint
tflint --recursive

# Install checkov for security scanning
pip install checkov

# Run checkov
checkov -d . --framework terraform
```

### Enable GitHub Security Features

1. **CodeQL scanning**
   - Go to: Security → Code scanning → Setup CodeQL
   - Choose: "Default" for quick setup
   - Or use Advanced for custom configuration

2. **Dependabot alerts**
   - Go to: Settings → Security & analysis
   - Enable: Dependabot alerts
   - Enable: Dependabot security updates

3. **Secret scanning**
   - Enable in: Settings → Security & analysis
   - Available for public repos, or private with GitHub Advanced Security

## Month 2: Error Tracking (2-3 hours)

### Setup Sentry (Recommended)

1. **Sign up at sentry.io** (free tier is generous)

2. **Create a new project** for your application

3. **Install SDK**

   For Node.js:
   ```bash
   npm install @sentry/node
   ```

   For Python:
   ```bash
   pip install sentry-sdk
   ```

   For Go:
   ```bash
   go get github.com/getsentry/sentry-go
   ```

4. **Initialize in your code**

   Node.js:
   ```javascript
   const Sentry = require("@sentry/node");
   Sentry.init({
     dsn: process.env.SENTRY_DSN,
     environment: process.env.NODE_ENV,
   });
   ```

   Python:
   ```python
   import sentry_sdk
   sentry_sdk.init(
       dsn=os.environ.get("SENTRY_DSN"),
       environment=os.environ.get("ENVIRONMENT"),
   )
   ```

5. **Store DSN in secrets**
   - Add to `.env.example`: `SENTRY_DSN=your-dsn-here`
   - Never commit actual DSN to git
   - Add to GitHub Secrets for CI/CD

## Ongoing: Best Practices

### Daily

- Review Dependabot PRs (approve or postpone)
- Check CI status before merging PRs
- Monitor error tracking dashboard

### Weekly

- Review security alerts
- Update one piece of technical debt
- Retrospective on blockers

### Monthly

- Review and update this documentation
- Assess impact of tools (time saved vs. time spent)
- Decide what to add/remove based on value

## Troubleshooting

### CI failing after setup

1. Check the Actions tab for detailed logs
2. Common issues:
   - Missing dependencies in CI environment
   - Test database not configured
   - Environment variables not set
3. Run tests locally first: `npm test` or `pytest`

### Pre-commit hooks too slow

1. Run only on changed files:
   ```bash
   # For husky/lint-staged
   npm install --save-dev lint-staged
   ```

2. Configure in `package.json`:
   ```json
   "lint-staged": {
     "*.js": "eslint --fix",
     "*.py": "black"
   }
   ```

### Team not using PR templates

1. Make them mandatory in branch protection
2. Add automated checks for required sections
3. Lead by example in your own PRs
4. Discuss benefits in team meeting

## Next Steps

After completing the basics:

1. Review [STARTUP_IMPROVEMENTS.md](STARTUP_IMPROVEMENTS.md) for Phase 2-4 items
2. Measure impact: track time saved, bugs caught, etc.
3. Gather team feedback on what's working
4. Iterate and improve based on actual usage

## Getting Help

- GitHub Actions: https://docs.github.com/actions
- Dependabot: https://docs.github.com/code-security/dependabot
- Sentry: https://docs.sentry.io
- Team discussions: Use issue #XXX (create one for tooling discussions)

## Resources

- [GitHub Skills](https://skills.github.com/) - Free interactive tutorials
- [Conventional Commits](https://www.conventionalcommits.org/)
- [Keep a Changelog](https://keepachangelog.com/)
- [Semantic Versioning](https://semver.org/)
