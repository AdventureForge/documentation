---
sidebar_position: 6
---

# CI/CD

For this project I have set up a full CI/CD pipeline for each repository.

## React application

##This CI is in 4 steps##

- Build and analysis by SonarCLoud
- Build and analysis by Github
- Docker image build and push to the Dockerhub
- The production server which run the dockerized application listen for new version on the Dockerhub, then properly shut down the app, pull, build and deploy the new image.

### workflow configuration

[configuration file](https://github.com/AdventureForge/af-react-app/blob/main/.github/workflows/build-push-dockerhub.yml)

### CI pipeline

The CI can be found here: [react applicaiton CI/CD](https://github.com/AdventureForge/af-react-app/actions)

### DockerHub

The images for the react app: [React app Docker repository](https://hub.docker.com/repository/docker/morganlmd/adventureforge-reactui)

## Spring cloud microservices

##This CI is in 3 steps##

- Build and analysis by SonarCLoud
- Parallel build of each microservice for which a need for a new build has been detected and push to the Dockerhub
- The production server which run the dockerized application listen for new version on the Dockerhub, then properly shut down the app, pull, build and deploy the new image.

### workflow configuration

[configuration file](https://github.com/AdventureForge/api/blob/main/.github/workflows/build-push-dockerhub.yml)

### CI pipeline

The CI can be found here: [Spring microservices CI/CD](https://github.com/AdventureForge/api/actions)

### DockerHub

- [Adventure service](https://hub.docker.com/repository/docker/morganlmd/adventureforge-adventureservice)
- [Game service](https://hub.docker.com/repository/docker/morganlmd/adventureforge-gameservice)
- [Gateway server](https://hub.docker.com/repository/docker/morganlmd/adventureforge-gatewayserver)
- [Config server](https://hub.docker.com/repository/docker/morganlmd/adventureforge-configserver)
- [Eureka server](https://hub.docker.com/repository/docker/morganlmd/adventureforge-eurekaserver)

## Docusaurus documentation

### workflow configuration

[configuration file](https://github.com/AdventureForge/documentation/blob/main/.github/workflows/deploy.yml)

### CI pipeline

The CI can be found here: [Docimentation applicaiton CI/CD](https://github.com/AdventureForge/documentation/actions)
