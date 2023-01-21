# jenks

Jenkins

## About

We need to build docker images inside a Jenkins container for the CI/CD process. In my research I came across two approaches: DooD (Docker-outside-of-Docker) and DinD (Docker-in-Docker).

DinD is where a complete and isolated version of Docker is installed inside a container.

The DooD appraoch involves mounting the host machine’s docker socket to the Jenkins container. With this, Jenkins can start new sibling containers. The sibling containers will run along side the Jenkins container on the host machine.

By adding the Docker package to the image and adding the Jenkins user to the Docker group, we provide the permissions and dependencies for Jenkins to run Docker commands. But, since Jenkins is running in a container, it does not have direct access to the host's Docker daemon. Mounting the host's Docker socket allows the container to communicate with the host's Docker daemon and thus Jenkins can run Docker commands.

## Setup

Build a custom image based off the official `jenkins/jenkins` image. This will include docker in the container. See [my Dockerfile](./Dockerfile).

```
docker build -t myjenkins-arm64 .
```

When running the container we mount a local folder, `jenkins`, on the host machine to the /var/jenkins_home directory within the Jenkins container. This is so that the data stored by Jenkins will persist across container restarts. I made this folder like so:

```
cd ~
```

```
mkdir jenkins
```

Start a docker container running Jenkins which has access to the host machine's docker daemon:

```
docker run -d -p 8080:8080 -p 50000:50000 -v jenkins:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock myjenkins-arm64
```

Install the `docker` plugin and the `docker-pipeline` plugin.

Then, you configure the `docker` plugin through `Manage Jenkins->Manage Nodes and Clouds->Configure Clouds->Add Docker`

- Docker Host URI: `unix:///var/run/docker.sock`
- Make sure you check 'Enabled'

To configure the `docker-pipeline` plugin through `Manage Jenkins->Global Tool Configuration->Add Docker`

- Docker version: latest
- Download from docker.com

## Jenkinsfile

If you want to test to see if it works, you can create a simple Maven-based Java project and add a Jenkins file at the root like:

```
// Jenkinsfile (Declarative Pipeline)
// Requires the Docker Pipeline plugin
// Requires the Docker plugin
pipeline {
    agent { docker { image 'maven:3.8.7-eclipse-temurin-11' } }
    stages {
        stage('build') {
            steps {
                sh 'mvn --version'
            }
        }
    }
}
```

Create a new multibranch pipeline for the repo (link the git repo to it)

---

## Ongoing Notes

I'd like to note that exposing your host docker socket is not advised. Also, DinD is a public archive now. I'm still looking into other alternatives to this. Most likely, we'd have to isolate the environment Jenkins is running in.

Like, get a VM, get docker on it, then run a jenkins container inside that VM with access to the VM's docker daemon. Or, just have a node dedicated to running that Jenkins container so that even if someone gained access the whole system isn't compromised.

- https://stackoverflow.com/questions/35110146/can-anyone-explain-docker-sock
- https://www.lvh.io/posts/dont-expose-the-docker-socket-not-even-to-a-container/
- https://dev.to/petermbenjamin/docker-security-best-practices-45ih#docker-engine
- https://blog.container-solutions.com/running-docker-in-jenkins-in-docker
- https://medium.com/@manav503/how-to-build-docker-images-inside-a-jenkins-container-d59944102f30
- https://jpetazzo.github.io/2015/09/03/do-not-use-docker-in-docker-for-ci/
