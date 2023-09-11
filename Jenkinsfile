pipeline{
    agent{
        label "jenkins-agent"
    }
    tools{
        jdk 'Java17'
        maven 'Maven3'
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
                withSonarQubeEnv(credentialsId: 'jenkins-sonarqube-token'){
                sh "mvn sonar:sonar"
                }
            }
        }
    }
}