**Welcome to Cloudsmith Support Assessment**

Please review the documentation provided by the recruiter for full instructions.

**Repository Overview**

This repository contains a simple Python package that can be built and published to Cloudsmith using GitHub Actions.

**Setup**

1. Clone the repository to your local machine
2. Create a new private repo in your Github namespace
3. Upload the code to the newly created Github repo
4. Start debugging!
5. Make sure to share access with the appointed people in the email
6. Include a markdown file which includes all 6 issues found 

**Build and Publish Process**

The process involves three workflows:

### 1. `build_package.yml`

* Triggered by a push or pull request to the main branch.
* Builds a Python package and saves it as an artifact in GitHub Actions.

### 2. `realease_package.yml`

* Triggered by the `build_package.yml` workflow completing successfully.
* Downloads the artifact from GitHub Actions and pushes it to the staging repository on Cloudsmith.

### 3. `promote_package.yml`

* Triggered manually by the repository maintainer.
* Promotes the package from the staging repository to the production repository on Cloudsmith.

**Authentication**

OIDC Authentication must be used for the project, API Key solutions will be rejected.



# CHANGES DONE BY YASH SHARMA
# Package Release Workflow Updates

This repository has undergone several changes to ensure a seamless package promotion and release process. Below are the steps taken to ensure that the package can be copied and released without any issues:

## Changes Summary

### 1. **Build Command in `build_package.yaml`**
   - The `build_package.yaml` file was missing the build command, which has now been added to ensure the correct build process is triggered during the release.

### 2. **ID Token in `release_package.yml`**
   - An ID token was missing in the `release_package.yml` file. This has been added to authenticate and authorize the release process correctly.

### 3. **OIDC Credentials in `release_package.yml`**
   - OIDC (OpenID Connect) credentials were added in the `release_package.yml` to ensure secure access to the necessary services during the release process.

### 4. **Fixed OIDC Credentials in `promote_package.yml`**
   - Fixed the OIDC credentials in `promote_package.yml` to ensure that the promotion process works securely and correctly.

### 5. **Fixed Versioning Issue in `pyproject.toml`**
   - There was an error in the `project` section of the `pyproject.toml` file where the version was not being read correctly.
   - This has been fixed by dynamically picking up the version from `__init__.py` by updating the `tool.hatch.version` path and setting the version to `dynamic`.

### 6. **Correct Repo Names in `promote_package.yml`**
   - The repository names in `promote_package.yml` were incorrect.
   - The correct names for staging and production repositories have been added:
     - `CLOUDSMITH_STAGING_REPO` for staging
     - `CLOUDSMITH_PRODUCTION_REPO` for production

### 7. **Added New GitHub Variables**
   - Added the following environment variables in GitHub to configure Cloudsmith repositories and namespaces:
     - `CLOUDSMITH_SERVICE_SLUG`
     - `CLOUDSMITH_NAMESPACE`

### 8. **Created Repositories and Services in Cloudsmith**
   - Created the necessary staging and production repositories in Cloudsmith.
   - Added the appropriate services in the access controls of the repositories to ensure that GitHub Actions has permission to write to the Cloudsmith repositories.

### 9. **Webhook Configuration for Package Promotion**
   - Made changes to ensure the webhook can successfully trigger the `promote_package.yml` workflow, ensuring automated package promotion.

## How to Use

1. **Ensure all variables are correctly set** in the GitHub repository settings under "Secrets" for the following variables:
   - `CLOUDSMITH_SERVICE_SLUG`
   - `CLOUDSMITH_NAMESPACE`
   - `CLOUDSMITH_STAGING_REPO`
   - `CLOUDSMITH_PRODUCTION_REPO`

2. **Verify the build and release workflows**:
   - The updated `build_package.yaml` will ensure that the package builds correctly during the CI/CD process.
   - The `release_package.yml` and `promote_package.yml` workflows are configured to handle package promotion from staging to production in Cloudsmith.

3. **Ensure the Cloudsmith repositories are accessible**:
   - GitHub Actions must have write permissions to the Cloudsmith repositories (staging and production).

4. **Webhook Integration**:
   - Ensure the webhook is set up and configured to trigger the promotion process.

## Conclusion

With these changes, the package release and promotion process should now be smooth and automated, ensuring minimal errors and consistent deployments between staging and production environments.
