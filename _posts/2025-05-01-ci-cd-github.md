---
title: CI/CD Using Github
date: 2025-05-01 00:00:00 +0000
description: Continuous integration and continuous deployment using Github
img: # Add image post (optional)
classes: wide
tags: [CI/CD, Github, Python] # add tag
---
# Continuous integration and continuous deployment

Continuous Integration (CI) and Continuous Deployment (CD) are key practices in modern software development that streamline the release process. CI involves frequently integrating code changes into a central repository and running automated tests to catch issues early. 
CD, on the other hand, focuses on the automated release of code changes to production after they pass through the CI process. 

We will explore the concept with sample example and easy to follow steps.

## Prerequisites
- Basic understanding of Git and GitHub
- Familiarity with Python programming language
- A GitHub account
- A basic understanding of CI/CD concepts
- A sample Python project (can be a simple FastAPI, Flask or Django app)
- A CI/CD tool (e.g., GitHub Actions, Travis CI, CircleCI) for automation
- Basic knowledge of Docker (optional, but recommended for containerization)

## Step 1: Set Up Your GitHub Repository
1. Create a new repository on GitHub for your Python project.
2. Clone the repository to your local machine using Git.
   ```bash
    git clone <repository_url>
    cd <repository_name>
   ```
3. Create a new branch for your CI/CD setup.
   ```bash
    git checkout -b ci-cd-setup
   ```
4. Add your Python project files to the repository.
    ```bash
    git add .
    git commit -m "Add Python project files"
    git push origin ci-cd-setup
    ```
5. Create a pull request to merge the `ci-cd-setup` branch into the `main` branch.
6. Review and merge the pull request on GitHub.
## Step 2: Create a CI/CD Configuration File
1. In the root of your repository, create a directory named `.github/workflows`.
2. Inside the `workflows` directory, create a YAML file (e.g., `ci-cd.yml`) for your CI/CD configuration.
3. For this example, we will use GitHub Actions as our CI/CD tool. The YAML file will define the workflow for building, testing, creating release and deploying your Python application.
4. Add the following sample configuration to the YAML file:
   ```yaml
    name: Python application CI, Build, Release, and Containerize

    on:
    push:
        branches: ["main"]
    pull_request:
        branches: ["main"]

    permissions:
    contents: write # Need write permission to create releases/tags and upload artifacts
    packages: write # Need write permission to push to GHCR

    jobs:
    # --- Build and Test Job ---
    build_and_test:
        runs-on: ubuntu-latest
        steps:
        - name: Checkout code
            uses: actions/checkout@v4

        - name: Set up Python 3.10
            uses: actions/setup-python@v5
            with:
            python-version: "3.10"
            cache: "pip"

        - name: Install Test Dependencies
            run: |
            python -m pip install --upgrade pip
            if [ -f requirements.txt ]; then
                pip install -r requirements.txt
            else
                echo "Dependency file not found!"
                exit 1
            fi

        - name: Test with pytest and check coverage
            run: |
            pytest --cov=src --cov=. --cov-report=term-missing --cov-report=xml --cov-fail-under=80    

    # --- Build and Push Docker Image Job ---
    build_and_push_docker:
        needs: build_and_test
        runs-on: ubuntu-latest
        steps:
        - name: Checkout code
            uses: actions/checkout@v4

        - name: Log in to GitHub Container Registry
            uses: docker/login-action@v2
            with:
            registry: ghcr.io
            username: ${{ github.actor }}
            password: ${{ secrets.GITHUB_TOKEN }}

        - name: Build Docker Image
            run: |
            docker build -t ghcr.io/${{ github.repository_owner }}/${{ github.repository }}:latest .

        - name: Push Docker Image
            run: |
            docker push ghcr.io/${{ github.repository_owner }}/${{ github.repository }}:latest

        - name: Output Docker Image Details
            id: docker_meta
            run: |
            echo "image_tag=ghcr.io/${{ github.repository_owner }}/${{ github.repository }}:latest" >> $GITHUB_ENV
            echo "image_digest=$(docker inspect --format='{{index .RepoDigests 0}}' ghcr.io/${{ github.repository_owner }}/${{ github.repository }}:latest)" >> $GITHUB_ENV
            env:
            GITHUB_ENV: $GITHUB_ENV

    # --- Create Release Job ---
    create_release:
        needs: build_and_push_docker
        runs-on: ubuntu-latest
        steps:
        - name: Checkout code
            uses: actions/checkout@v4

        - name: Create Release
            uses: actions/create-release@v1
            env:
            GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
            with:
            tag_name: "v${{ github.run_number }}"
            release_name: "Release v${{ github.run_number }}"
            body: |
                Automated release created by CI/CD workflow.
                - Docker Image: ghcr.io/${{ github.repository_owner }}/${{ github.repository }}:latest
            draft: false
            prerelease: false

    # --- Deploy Job ---
    deploy:
        needs: build_and_push_docker
        runs-on: self-hosted
        steps:
        - name: Checkout code
            uses: actions/checkout@v4

        - name: Deploy Code to Runner
            run: |
            mkdir -p /home/user/deployed_app
            rsync -av --progress ./ /home/user/deployed_app --exclude .git --exclude .github --exclude .gitignore
            nohup fastapi run --reload &
            echo "Code deployed to /home/user/deployed_app"
   ```
5. This configuration defines two jobs: `build_and_test`, `build_and_push_docker`, `create_release` and `deploy`. The `build_and_test` job installs dependencies and runs tests, `build_and_push_docker` job build docker image and push it to github container registry by tagging with gitrepo, `create_release` creates release with autogenerated version number and image details, and `deploy` deploys the application to production using github runner.
6. Github runner is a self-hosted runner that you need to set up on your server. You can follow the [GitHub documentation](https://docs.github.com/en/actions/hosting-your-own-runners/about-self-hosted-runners) to set up a self-hosted runner.
6. Customize the deployment step according to your deployment strategy (e.g., SSH, Docker, etc.).
## Step 3: Commit and Push Changes
1. Commit the changes to your `.github/workflows/ci-cd.yml` file.
   ```bash
   git add .github/workflows/ci-cd.yml
   git commit -m "Add CI/CD configuration"
   git push origin main
   ```
2. This will trigger the CI/CD pipeline defined in the YAML file.
3. You can monitor the progress of the pipeline in the "Actions" tab of your GitHub repository.
## Step 4: Monitor and Debug
1. If the pipeline fails, check the logs for each step to identify the issue.
2. Common issues may include missing dependencies, test failures, or deployment errors.
3. Fix the issues in your code and commit the changes to trigger the pipeline again.
4. Once the pipeline succeeds, your application will be deployed to production automatically.
## Conclusion
In this tutorial, we explored how to set up a CI/CD pipeline using GitHub Actions for a Python project. We created a GitHub repository, configured the CI/CD pipeline, and monitored its execution. By automating the build, test, and deployment processes, we can ensure that our code is always in a deployable state, leading to faster and more reliable software releases.
## Additional Resources
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Continuous Integration and Continuous Deployment](https://www.atlassian.com/continuous-delivery/ci-cd)
- [Python Testing Documentation](https://docs.python.org/3/library/unittest.html)
- [Docker Documentation](https://docs.docker.com/)
- [FastAPI Documentation](https://fastapi.tiangolo.com/)
- [Flask Documentation](https://flask.palletsprojects.com/)
- [Django Documentation](https://www.djangoproject.com/)
- [Travis CI Documentation](https://docs.travis-ci.com/)
- [CircleCI Documentation](https://circleci.com/docs/)
- [Git Documentation](https://git-scm.com/doc)
- [GitHub Documentation](https://docs.github.com/en)
- [Python Documentation](https://docs.python.org/3/)
- [GitHub Actions Marketplace](https://github.com/marketplace?type=actions)
- [GitHub Actions Examples](https://github.com/actions)
- [GitHub Actions CI/CD](https://github.com/actions)
