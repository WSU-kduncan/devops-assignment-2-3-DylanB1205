# DevOps Assigment 2 - GitHub Actions & DockerHub

## Description of Workflow
- This workflow builds a java app. with gradle, creates the docker image and then pushes it to my docker hub account. Lines 4-8 triggers the workflow on push to the main brain, 

on:
  push:
    branches: [ "main" ]

- We then set permissions within the repo to read only, and build the application by specifying what its running on and installing a jdk.

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-java@v3
        with:
          distribution: temurin
          java-version: 17

- After building the image and installing a jdk (in our instance java), we run the command to build with execute commands and chmod, alongisde copying and renaming the jar file.

name: Execute Gradle build
        run: |
          chmod +x ./gradlew
          ./gradlew build
name: Copy Jar file
run: mv ./build/libs/`*`.jar ./app.jar

- Lastly we build and push the image using ubuntu, and login to to docker hub and commit to our dockerhub repo.

build-image:
    runs-on: ubuntu-latest
    needs: build
    steps:
      - uses: actions/download-artifact@master
        with:
          name: job-dependencies
          path: .

 name: Login to Docker Hub
        uses: docker/login-action@v2
        with:
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}

push: true
          tags: dbrum12/brumbaugh-wopro-service:${{ github.sha }}


## Link to DockerHub Repository
[DockerHub - `YOURLASTNAME-WOPro-Service`](https://hub.docker.com/repository/docker/dbrum12/brumbaugh-wopro-service/general)

## Link to GitHub Actions Run Results Summary
[Link to **working** workflow run results](https://github.com/WSU-kduncan/devops-assignment-2-3-DylanB1205/actions/runs/11630598622/job/32389919628)
