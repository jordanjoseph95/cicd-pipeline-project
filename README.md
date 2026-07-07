# CI/CD Pipeline with GitHub Actions and ECR

This project builds a small Flask API, packages it with Docker, and deploys it automatically to AWS ECR using GitHub Actions. Every push to main triggers the pipeline. No manual docker commands needed after setup.

## What it does

- Runs a Flask app with two endpoints: a home route and a health check
- Packages the app into a Docker image using a Dockerfile
- Pushes the image to an AWS ECR repository
- Automates the build and push process with a GitHub Actions workflow
- Tags each image with the commit SHA and also updates a latest tag

## Why I built it this way

I pushed the image to ECR manually first. That way I understood exactly what the automation needed to do before I wrote the workflow file. GitHub Actions now handles the same steps I ran by hand: build, tag, push.

Tagging with the commit SHA means I can trace any image back to the exact code that built it. The latest tag stays there for convenience, so anyone pulling the image without specifying a version still gets the newest build.

## How to run this

1. Clone the repo and move into the folder
2. Install Docker and the AWS CLI
3. Run `aws configure` and add your credentials
4. Build the image locally with `docker build -t cicd-pipeline-app .`
5. Run it with `docker run -p 5001:5001 cicd-pipeline-app`
6. Visit `localhost:5001` and `localhost:5001/health` to confirm it works

To trigger the automated pipeline, push any change to main. GitHub Actions builds and pushes a new image to ECR using the AWS credentials stored as repository secrets.

## What's next

Right now the pipeline stops at ECR. The image builds and pushes automatically, but nothing deploys it anywhere yet. Adding an automatic deploy step to EC2 or ECS is the natural next step for this project.

