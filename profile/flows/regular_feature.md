## Extended Git Flow: Regular Feature Workflow

### 1. Initiate Release Branch
- **Action**: Checkout a new release branch from the `main` branch.
- **Purpose**: This branch serves as the foundation for features planned for the next release.

### 2. Feature Branch Creation
- **Action**: From the release branch, checkout a feature branch.
- **Purpose**: Develop specific features or changes independently of the main development line.

### 3. Development and Pull Request
- **Action**: Commit and push all changes on the feature branch. Then, create a pull request from the feature branch to the release branch.
- **Purpose**: Facilitates code reviews and integration testing before merging to the release branch.

### 4. Testing on Environment
- **Action**: Perform manual testing of the features on a test environment.
- **Purpose**: Ensure the new features function as expected under controlled test conditions.

### 5. Iterative Fixes
- **Action**: If unexpected behavior is detected, return to the feature branch to implement fixes. Submit another pull request to the release branch after adjustments, followed by additional testing.
- **Purpose**: Maintain high standards of quality and functionality by addressing issues prior to release.

### 6. Preparing for Release
- **Action**: Create a pull request from the release branch to the `main` branch once all features are finalized and tested.
- **Purpose**: Prepares the codebase for a new release by integrating all new features into the main line.

### 7. Tagging and GitHub Release
- **Action**: After merging to the main branch, create a tag from the main branch to define the release version. Use this tag to create a GitHub release and review the changelog.
- **Purpose**: Officially mark the release point in the repository and provide detailed version history through changelogs.

### 8. Merge Back to Develop
- **Action**: Merge the main branch back into the `develop` branch.
- **Purpose**: Keeps the development branch updated with all the changes that have been released, ensuring consistency across all branches.
