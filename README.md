# Jenkins Go CI/CD Pipeline 

This project demonstrates a complete CI/CD pipeline using **Jenkins**, **Docker**, and a simple **Go** application.

## Tech Stack

- **Jenkins (Blue Ocean)** – for orchestrating builds and pipeline execution
- **Docker** – for containerizing the Jenkins controller and custom Go agent
- **Go (Golang)** – sample application language
- **GitHub** – for version control and SCM polling
  


## Sample Go Application

The Go app simply prints:

```go
fmt.Println("Hello, World!")
```

## Jenkinsfile (Pipeline Logic)

The pipeline consists of:

1. **Build** – Installs dependencies and compiles the Go app.
2. **Test** – Runs the compiled Go binary.
3. **Deliver** – Placeholder step for deployment.

```groovy
pipeline {
    agent {
        node {
            label 'docker-agent-go'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh '''
                cd myapp
                go mod tidy
                go build -o hello gohello.go
                '''
            }
        }
        stage('Test') {
            steps {
                sh '''
                cd myapp
                ./hello
                '''
            }
        }
        stage('Deliver') {
            steps {
                sh 'echo Delivery successful!'
            }
        }
    }
}
```

## Custom Go Jenkins Agent

The custom agent includes:

- Go 1.20
- OpenJDK 17
- `agent.jar` for Jenkins inbound connection

```dockerfile
FROM golang:1.20

RUN apt-get update && \
    apt-get install -y curl git openjdk-17-jdk && \
    apt-get clean

WORKDIR /go

RUN curl -o agent.jar http://host.docker.internal:8080/jnlpJars/agent.jar

ENTRYPOINT ["java", "-jar", "agent.jar", "-url", "http://host.docker.internal:8080", "-name", "docker-agent-go"]
```

## DockerHub

Agent image published at:  
https://hub.docker.com/r/404yeti/jenkins-go-agent

## Lessons Learned

- How to build a Jenkins pipeline from scratch
- How to write and push Docker images to Docker Hub
- How to use GitHub + Jenkins for polling and automation
- How to integrate Go builds in CI/CD workflows

## Next Steps

- Add automated tests
- Push Go binary to an artifact repo
- Integrate Grafana for monitoring metrics
- Add Slack or email notifications for pipeline status

---

*Built by [404Yeti](https://github.com/404Yeti) to master DevOps pipelines!*
