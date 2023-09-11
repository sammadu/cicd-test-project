pipeline{
    agent{
        label "jenkins-agent"
    }
    tools{
        jdk 'Java17'
        maven 'Maven3'
    }
    environment{
        APP_NAME = "cicd-test-project"
        RELEASE = "1.0.0"
        DOCKER_USER = "samiyamax"
        DOCKER_PASS = "dockerhub-access-token"
        IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
        IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"

    }
    stages{
        stage("Cleanup Workspace"){
            steps{
                cleanWs()
            }
        }
        stage("Checkout from SCM"){
            steps{
                git branch: 'master', credentialsId: 'jenkins-fine-grained-key', url: 'https://github.com/sammadu/cicd-test-project.git'
            }
        }

        stage("Build Application"){
            steps{
                sh "mvn clean package"
            }
        }

        stage("Test Application"){
            steps{
                sh "mvn test"
            }
        }

        stage("Sonarqube Analysis"){
            steps{
                script {
                withSonarQubeEnv(credentialsId: 'jenkins-sonarqube-token') {
                sh "mvn sonar:sonar"
                }
                }
            }
        }

        stage("Quality Gate"){
            steps{
                waitForQualityGate abortPipeline: false, credentialsId: 'jenkins-sonarqube-token'
            }
        }

        stage("Build & Push Docker Image"){
            steps{
                script{
                    docker.withRegistry('',DOCKER_PASS) {
                        docker_image = docker.build "${IMAGE_NAME}"
                    }

                    docker.withRegistry('',DOCKER_PASS) {
                        docker_image.push("$(IMAGE_TAG)")
                        docker_image.push('latest')
                    }
                }
            }
        }
    }
}