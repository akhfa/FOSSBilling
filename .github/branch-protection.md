# Branch Protection Configuration for SonarCloud Integration

This document outlines the branch protection rules that should be configured in the GitHub repository settings to enforce SonarCloud quality gates.

## Repository Settings Configuration

Navigate to **Settings > Branches** in your GitHub repository and configure the following branch protection rules for the `main` branch:

### Required Status Checks

Enable the following status checks as required before merging:

1. **SonarCloud Analysis / SonarCloud Analysis** - The main SonarCloud analysis job
2. **SonarCloud Analysis / SonarCloud PR Quality Gate** - The quality gate check for PRs
3. **CI / PHP Build and Test** - Existing CI checks should remain required

### Additional Settings

- ✅ **Require status checks to pass before merging**
- ✅ **Require branches to be up to date before merging**
- ✅ **Require pull request reviews before merging** (minimum 1 review)
- ✅ **Dismiss stale reviews when new commits are pushed**
- ✅ **Require review from code owners** (if CODEOWNERS file exists)
- ✅ **Restrict pushes that create files that exceed 100 MB**
- ✅ **Require signed commits** (optional, for enhanced security)

### Quality Gate Rules

The SonarCloud quality gate will enforce the following default conditions:
- **Coverage**: Minimum 80% code coverage on new code
- **Duplicated Lines**: Less than 3% duplication on new code
- **Maintainability Rating**: A rating on new code
- **Reliability Rating**: A rating on new code
- **Security Rating**: A rating on new code
- **Security Hotspots**: All security hotspots reviewed

## Manual Setup Steps

1. **SonarCloud Project Setup**:
   - Go to https://sonarcloud.io/
   - Import the FOSSBilling/FOSSBilling repository
   - Set the project key as `FOSSBilling_FOSSBilling`
   - Set the organization as `fossbilling`

2. **GitHub Secrets Configuration**:
   Add the following secret to the repository:
   - `SONAR_TOKEN`: Generate this token from your SonarCloud account

3. **Quality Gate Customization**:
   - In SonarCloud, navigate to your project
   - Go to **Administration > Quality Gate**
   - Customize conditions as needed for your project requirements

## Webhook Configuration (Optional)

For real-time status updates, you can configure a webhook:
- **Payload URL**: `https://api.github.com/repos/FOSSBilling/FOSSBilling/statuses/{sha}`
- **Content type**: `application/json`
- **Events**: Push, Pull requests

## Troubleshooting

### Common Issues:
1. **SONAR_TOKEN not found**: Ensure the secret is properly configured in repository settings
2. **Quality Gate timeout**: Increase timeout in workflow or optimize analysis scope
3. **Coverage reports missing**: Verify PHPUnit configuration generates coverage.xml
4. **Branch protection conflicts**: Ensure all required checks are properly named and enabled

### Support:
- SonarCloud Documentation: https://docs.sonarcloud.io/
- GitHub Branch Protection: https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/defining-the-mergeability-of-pull-requests/about-protected-branches