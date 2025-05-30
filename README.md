# Jenkins CI/CD Pipeline for GitHub Repository

This repository is configured to use a Jenkins multibranch pipeline for continuous integration and automated testing. The pipeline runs tests on every branch and pull request (PR) and reports the build status directly on GitHub.

## Infrastructure Overview

- **Jenkins Host**: AWS EC2 instance running Ubuntu
- **CI Tool**: Jenkins (installed manually on EC2)
- **Pipeline**: Defined using a `Jenkinsfile` stored in the repository
- **GitHub Integration**: Status checks shown in pull requests
- **Containerized Builds**: Each build runs in a Docker container using Python 3.10

## Features

- **Multibranch Pipeline**: Jenkins automatically discovers branches and pull requests from GitHub.
- **Automated Testing**: Runs tests using `pytest` inside a container.
- **Status Reporting**: Shows success or failure directly in GitHub PRs as checks.
- **Workspace Cleanup**: Ensures a clean workspace before each build.
- **Requirements Installation**: Installs project dependencies using pip.

## How It Works

1. Jenkins periodically scans the repository for new branches and PRs.
2. When a commit is pushed or a PR is opened/updated, Jenkins triggers a build.
3. Jenkins executes the following pipeline:
    - Cleans the workspace
    - Clones the repo using the current branch
    - Installs dependencies from `requirements.txt`
    - Runs tests using `pytest`
4. The result (success or failure) is sent back to GitHub and displayed in the PR as a check.

## Requirements

- AWS EC2 instance running **Ubuntu** with Jenkins and Docker installed
- Jenkins Plugins:
  - GitHub Branch Source Plugin
  - GitHub Integration Plugin
  - GitHub Plugin
- GitHub personal access token (PAT) with `repo:status` scope
- PAT configured as a Jenkins credential (`credentialsId: 'github-token'`)
- Jenkins must be publicly accessible (or tunneled) for GitHub webhooks

## Setting Up the GitHub Webhook

To allow GitHub to notify Jenkins of changes:

1. Go to your GitHub repo → **Settings > Webhooks**
2. Click **Add webhook**
3. Use the following settings:
   - **Payload URL**: `http://<your-public-jenkins-url>/github-webhook/`
   - **Content type**: `application/json`
   - **Events**: Choose "Let me select individual events" and select:
     - `Pushes`
     - `Pull requests`
4. Save the webhook

## GitHub Pull Request Example

When a PR is opened or updated, GitHub shows a check under "Checks":


This ensures that code is tested before being merged.

## Notes

- Make sure port 8080 is open in your EC2 security group if Jenkins is running on that port.
- You can configure HTTPS with a reverse proxy like Nginx + Certbot for a production setup.
- Modify the Jenkinsfile as needed to fit your tech stack.

---

**Author:** Ashley Pérezz  
**Project:** Jenkins CI/CD Test on EC2 Ubuntu  
**Repository:** [ashleyprz/jenkins-prueba](https://github.com/ashleyprz/jenkins-prueba)
