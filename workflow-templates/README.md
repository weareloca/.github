# GitHub Actions Workflow Templates

This directory contains CI/CD workflow templates tailored for our tech stack: Laravel, NestJS, Next.js, Terraform, and AWS Lambda/SAM.

## Available Templates

### 1. Node.js CI (`node-ci.yml`)
**For**: NestJS and Next.js applications

**Features**:
- Runs on multiple Node.js versions (18.x, 20.x)
- Code formatting check with Prettier
- Linting with ESLint
- TypeScript type checking
- Unit tests with coverage
- Build verification
- Coverage upload to Codecov

**Setup**:
```bash
cp workflow-templates/node-ci.yml .github/workflows/ci.yml
```

**Requirements**:
- `package.json` with scripts: `lint`, `test`, `build`
- Optional: `type-check` script for TypeScript projects

---

### 2. Laravel CI (`laravel-ci.yml`)
**For**: Laravel PHP applications

**Features**:
- Runs on multiple PHP versions (8.2, 8.3)
- MySQL and Redis services for testing
- Code style check with Laravel Pint
- Static analysis with PHPStan
- Database migrations
- PHPUnit/Pest tests with coverage
- Coverage upload to Codecov

**Setup**:
```bash
cp workflow-templates/laravel-ci.yml .github/workflows/ci.yml
```

**Requirements**:
- Laravel 9+ (includes Pint)
- PHPStan: `composer require --dev phpstan/phpstan`
- `.env.example` file for CI environment

**Customization**:
- Adjust PHP versions in matrix
- Change database from MySQL to PostgreSQL if needed
- Update PHPStan level (currently set to 5)
- Adjust coverage threshold (currently 70%)

---

### 3. Terraform CI (`terraform-ci.yml`)
**For**: Terraform infrastructure as code

**Features**:
- Runs on multiple Terraform versions (1.6.x, 1.7.x)
- Format checking (`terraform fmt`)
- Terraform validation
- TFLint for best practices
- Checkov security scanning
- Plan generation (with PR comments)

**Setup**:
```bash
cp workflow-templates/terraform-ci.yml .github/workflows/terraform-ci.yml
```

**Requirements**:
- Valid Terraform configuration files
- Optional: AWS credentials for `terraform plan`

**Customization**:
- Add backend configuration for `terraform init`
- Configure AWS credentials for plan/apply
- Adjust Checkov severity threshold

---

### 4. AWS Lambda/SAM CI (`lambda-sam-ci.yml`)
**For**: AWS Lambda functions with SAM (Serverless Application Model)

**Features**:
- SAM CLI setup
- Node.js support (configurable for Python)
- Dependency installation
- Linting and testing
- SAM build and validate
- Optional: SAM local testing
- Optional: Automatic deployment from main branch

**Setup**:
```bash
cp workflow-templates/lambda-sam-ci.yml .github/workflows/lambda-ci.yml
```

**Requirements**:
- `template.yaml` (SAM template)
- Optional: `samconfig.toml` for deployment configuration
- AWS credentials (for deployment)

**Customization**:
- Uncomment Python setup for Python Lambda functions
- Enable SAM local testing if you have integration tests
- Enable automatic deployment by uncommenting the deploy step
- Add AWS credentials as GitHub Secrets

---

## General Usage

### 1. Copy Template
```bash
# Choose the appropriate template
mkdir -p .github/workflows
cp workflow-templates/[template-name].yml .github/workflows/ci.yml
```

### 2. Customize for Your Project
Edit the copied workflow file to:
- Adjust version matrices
- Update test commands
- Add environment variables
- Configure coverage thresholds
- Add deployment steps (if needed)

### 3. Add Required Secrets
Go to GitHub repo → Settings → Secrets and variables → Actions

Common secrets needed:
- `AWS_ACCESS_KEY_ID` (for AWS deployments)
- `AWS_SECRET_ACCESS_KEY` (for AWS deployments)
- `AWS_REGION` (for AWS deployments)
- `CODECOV_TOKEN` (optional, for private repos)

### 4. Enable Branch Protection
Go to GitHub repo → Settings → Branches → Add rule

Recommended settings:
- ✅ Require a pull request before merging
- ✅ Require approvals (at least 1)
- ✅ Require status checks to pass before merging
- ✅ Select your CI workflow(s)
- ✅ Require branches to be up to date before merging

---

## Common Customizations

### Running on Specific Branches
```yaml
on:
  pull_request:
    branches: [ main, develop, 'release/**' ]
  push:
    branches: [ main, develop ]
```

### Running on Specific Paths
```yaml
on:
  pull_request:
    paths:
      - 'src/**'
      - 'tests/**'
      - 'package.json'
```

### Adding Environment Variables
```yaml
- name: Run tests
  run: npm test
  env:
    DATABASE_URL: postgresql://user:pass@localhost:5432/test
    REDIS_URL: redis://localhost:6379
```

### Multiple Jobs
```yaml
jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm run lint
  
  test:
    runs-on: ubuntu-latest
    needs: lint  # Wait for lint to pass
    steps:
      - uses: actions/checkout@v4
      - run: npm test
```

### Caching Dependencies
```yaml
# NPM cache (already included in node-ci.yml)
- uses: actions/setup-node@v4
  with:
    cache: 'npm'

# Composer cache (already included in laravel-ci.yml)
- uses: actions/cache@v3
  with:
    path: vendor
    key: ${{ runner.os }}-composer-${{ hashFiles('**/composer.lock') }}
```

---

## Troubleshooting

### Workflow Not Running
- Check if workflow file is in `.github/workflows/` directory
- Verify YAML syntax (use a YAML validator)
- Check repository settings → Actions → General → Allow all actions

### Tests Failing in CI but Passing Locally
- Check environment variables
- Verify database/service versions match
- Check timezone differences
- Look for file path issues (case sensitivity on Linux)

### Slow Workflows
- Enable dependency caching (already included in templates)
- Use `--frozen-lockfile` or `--no-optional` for faster installs
- Run jobs in parallel when possible
- Consider running heavy jobs only on main branch

### Coverage Upload Failing
- Codecov token not needed for public repos
- For private repos, add `CODECOV_TOKEN` to secrets
- Check coverage file path matches upload configuration

---

## Best Practices

1. **Start Simple**: Begin with basic CI, add complexity gradually
2. **Test Locally**: Use `act` to test workflows locally: https://github.com/nektos/act
3. **Monitor Costs**: GitHub Actions is free for public repos, but has limits for private repos
4. **Keep Secrets Secret**: Never commit credentials, use GitHub Secrets
5. **Fail Fast**: Put quick checks (lint, format) before slow ones (tests, build)
6. **Use Matrix Strategically**: Test on multiple versions only if you support them
7. **Cache Everything**: Enable caching for dependencies to speed up workflows
8. **Review Logs**: Check failed workflow logs in GitHub Actions tab

---

## Resources

- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Workflow Syntax](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions)
- [GitHub Actions Marketplace](https://github.com/marketplace?type=actions)
- [Laravel Testing](https://laravel.com/docs/testing)
- [NestJS Testing](https://docs.nestjs.com/fundamentals/testing)
- [Next.js Testing](https://nextjs.org/docs/testing)
- [Terraform Testing](https://developer.hashicorp.com/terraform/tutorials/configuration-language/test)
- [SAM Local Testing](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-using-start-api.html)
