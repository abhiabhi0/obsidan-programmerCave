### 1. **Writing a Feature**
- **Developer writes the code:** A developer writes a new feature or makes changes to the existing code in a feature branch.
- **Local Testing:** The developer tests the feature locally to ensure it works as expected.

### 2. **Raising a Pull Request (PR)**
- **Commit Changes:** The developer commits their changes to the feature branch.
- **Push to Remote Repository:** The changes are pushed to the remote repository (e.g., GitHub, GitLab).
- **Create PR:** The developer creates a Pull Request (PR) from the feature branch to the `main` branch. The PR includes a description of the changes and any relevant context or screenshots.

### 3. **Code Review and Approval**
- **Code Review:** Team members review the PR, provide feedback, and request changes if necessary.
- **Address Feedback:** The developer addresses any feedback and updates the PR accordingly.
- **Approval:** Once the PR is satisfactory, it is approved by the reviewers.

### 4. **Merging the PR**
- **Merge to Main:** The PR is merged into the `main` branch. This step triggers the CI/CD pipeline.

### 5. **CI/CD Pipeline**
- **Trigger CI/CD Pipeline:** Merging the PR to the `main` branch triggers the CI/CD pipeline configured in your CI/CD tool (e.g., Jenkins, GitHub Actions, GitLab CI).
  
  #### Steps in CI/CD Pipeline:
  1. **Build Docker Image:**
     - The pipeline script builds a new Docker image for the service using the updated code.
     - The image is tagged (usually with the commit SHA or a version number) and pushed to the Uniphore Docker registry.
  
  2. **Run Tests:**
     - Unit tests, integration tests, and other automated tests are executed to ensure the code quality and functionality.
  
  3. **Update Helm Chart Version:**
     - The Helm chart version is updated to reflect the new release. This can be automated using a script that updates the version in the `Chart.yaml` file and commits the change.
  
  4. **Push Helm Chart:**
     - The updated Helm chart is pushed to the Helm repository.
  
  5. **Deploy to EKS using FluxCD:**
     - FluxCD monitors the Git repository (where the Helm charts are stored).
     - FluxCD detects the new Helm chart version and deploys the updated service to the EKS production cluster.

### 6. **Deployment to EKS**
- **FluxCD Synchronization:**
  - FluxCD periodically checks the repository for changes.
  - Upon detecting the updated Helm chart, FluxCD applies the new version to the EKS cluster.
  
- **Helm Deployment:**
  - Helm installs or upgrades the service in the EKS cluster using the updated Helm chart.
  - Kubernetes resources (such as Deployments, Services, ConfigMaps, etc.) are created or updated accordingly.

### 7. **Post-Deployment Verification**
- **Monitor Deployment:**
  - The deployment is monitored to ensure it is successful. This includes checking pod status, logs, and application health.
- **Smoke Testing:**
  - Perform smoke tests or basic functionality tests to confirm the service is working as expected in the production environment.
- **Rollback (if needed):**
  - If any issues are found, a rollback can be performed to revert to the previous stable version. This can be automated or done manually using Helm.

### 8. **Release Notification**
- **Notify Stakeholders:**
  - Once the deployment is successful, notify relevant stakeholders (e.g., via email, Slack, or a project management tool).
- **Update Documentation:**
  - Update any relevant documentation to reflect the changes made in this release.

This workflow ensures a streamlined and automated process from development to deployment, minimizing manual intervention and reducing the chances of errors.