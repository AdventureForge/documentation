---
sidebar_position: 7
---

# Quality & Testing

This page presents to quality policy used for the project

## React application

### On the developper environment

#### Prettier

Prettier format the code automatically and ensure that the project is always well formatted according to the community standards.

#### EsLint

is a library that analyze the code in order to find bugs, anti pattern, security issues, unusued imports, variables and so on. It also automatically failed the deployement if the most obvious bugs are not fixed.

#### Typescript

### CI/CD

#### Github actions

The Continuous integration pipeline build the project and check that the project has no errors.

#### Sonar Cloud

The project is sent to Sonar Cloud which will analyze if there is still security issues, code smells, duplication etc. Sonar Cloud rejects the build if there are too many issues and fail the CI pipeline and the deployment. Which force me to take the feedbacks into account and fix the issues identified.

## Spring cloud microservices

### On the developper environment

#### Sonarlint

Sonarlint is a static analysis tool which analyse the code the same way EsLint does and report the errors or bad code.

### CI/CD

#### Github actions & SonarCloud

Similar setup than for the react app which build checking during the CI pipeline and then quality checking by sonarcloud.
