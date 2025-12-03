# CI/CD Pipeline Instructions

This repository is now equipped with a GitHub Actions CI/CD pipeline.

## 1. Pipeline Structure
The workflow is defined in `.github/workflows/ci-cd.yml` and consists of three jobs:
1.  **Frontend (Next.js)**: Installs dependencies, runs linting, and builds the Next.js application in `dss-atlas`.
2.  **Python Scripts Check**: Installs Python dependencies (`shapely`) and checks the syntax of `generate_patta_dataset.py`.
3.  **Deploy**: Runs only on pushes to `main` after successful build and test jobs.

## 2. Setup Requirements

### Secrets
Go to **Settings > Secrets and variables > Actions** in your GitHub repository and add the following secrets:

- `AWS_ACCESS_KEY_ID`: Your AWS IAM User Access Key.
- `AWS_SECRET_ACCESS_KEY`: Your AWS IAM User Secret Key.
- `AWS_REGION`: The AWS region where your Elastic Beanstalk environment is (e.g., `us-east-1`).
- `EB_APPLICATION_NAME`: The name of your Elastic Beanstalk Application.
- `EB_ENVIRONMENT_NAME`: The name of your Elastic Beanstalk Environment.
- `NEXT_PUBLIC_API_KEY`: (Optional) If your build requires API keys.

### AWS Setup Guide
1.  **Create IAM User**:
    - Go to AWS IAM Console -> Users -> Create user.
    - Attach policies: `AWSElasticBeanstalkWebTier`, `AWSElasticBeanstalkManagedUpdatesCustomerRolePolicy`, and `AWSElasticBeanstalkMulticontainerDocker` (or similar full access for EB).
    - Generate Access Keys for this user.
2.  **Create Elastic Beanstalk Application**:
    - Go to AWS Elastic Beanstalk Console -> Create Application.
    - Platform: **Node.js**.
    - Create the environment.
3.  **Configure Environment**:
    - Ensure your environment is running Node.js 20 (matching the workflow).
    - Set environment variables in Configuration -> Software.

### Python Dependencies
The current workflow manually installs `shapely`. If you add more Python dependencies:
1.  Create a `requirements.txt` in `dss-atlas/scripts/`.
2.  Update `.github/workflows/ci-cd.yml` to use `pip install -r requirements.txt`.

## 3. Triggering the Pipeline
- **Push to main**: Triggers the full pipeline including deployment.
- **Pull Request**: Triggers build and test jobs to verify changes before merging.

## 4. Artifacts
After a successful run, you can download the `nextjs-build` artifact from the Actions tab to inspect the build output.
