## Hotfix/Bugfix Workflow Without Testing on Test Environment

This workflow is designed for urgent fixes that need to be deployed immediately without the standard testing phase on a test environment. This approach should be used sparingly to ensure system stability.

### 1. Checkout Main Branch
- **Action**: Ensure you're starting from the most stable version of the project by checking out the `main` branch.
- **Purpose**: To apply the hotfix or bugfix from the most recent stable release.
- **Git Command**:
  ```bash
  git checkout main
  git pull origin main
  ```

### 2. Create Hotfix/Bugfix Branch
- **Action**: Checkout a new branch from the `main` branch dedicated to the hotfix or bugfix.
- **Purpose**: To isolate changes and ensure that the fix does not include unintended modifications.
- **Git Command**:
  ```bash
  git checkout -b hotfix/PDP-123-awesome_hotfix_for_the_production
  ```

### 3. Implement and Commit Changes
- **Action**: Make necessary changes to address the critical issue.
- **Purpose**: To quickly resolve the defect impacting the system's operation.
- **Git Command**:
  ```bash
  git add .
  git commit -m "fix: resolve critical issue affecting X functionality"
  git push origin hotfix/PDP-123-awesome_hotfix_for_the_production
  ```

### 4. Create Pull Request
- **Action**: Open a pull request from the `hotfix/PDP-123-awesome_hotfix_for_the_production` branch to the `main` branch.
- **Purpose**: To undergo a final review before merging, ensuring the fix is appropriate and does not introduce new issues.

### 5. Create Tag and GitHub Release
- **Action**: After merging the PR to the `main` branch, tag the commit and create a GitHub release.
- **Purpose**: To document the new state of the product post-fix and maintain a clear release history.
- **Git Command**:
  ```bash
  git checkout main
  git pull origin main
  git tag -a v1.0.1 -m "Hotfix release version 1.0.1"
  git push origin v1.0.1
  # Use GitHub UI to create the release based on the tag
  ```

### 6. Merge Changes to Current Release Branch
- **Action**: Merge the changes from the `main` branch into the current release branch.
- **Purpose**: To ensure that the current release also includes the urgent fixes.
- **Git Command**:
  ```bash
  git checkout main
  git pull origin main
  git checkout release/v1.1.0
  git pull origin release/v1.1.0
  git merge main
  git push origin release/v1.1.0
  ```

### 7. Update Develop Branch
- **Action**: Merge the current release branch into the `develop` branch.
- **Purpose**: To keep the development line up to date with all fixes and changes made in the current release.
- **Git Command**:
  ```bash
  git checkout release/v1.1.0
  git pull origin release/v1.1.0
  git checkout develop
  git pull origin develop
  git merge release/v1.1.0
  git push origin develop
  ```