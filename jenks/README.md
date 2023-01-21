# jenks

Jenkins

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

I'd like to note that exposing your host docker socket is not advised. I'm still looking into other alternatives to this.

- https://stackoverflow.com/questions/35110146/can-anyone-explain-docker-sock
- https://www.lvh.io/posts/dont-expose-the-docker-socket-not-even-to-a-container/
- https://dev.to/petermbenjamin/docker-security-best-practices-45ih#docker-engine
- https://blog.container-solutions.com/running-docker-in-jenkins-in-docker
