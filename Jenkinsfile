pipeline
    agent{
        label "jenkins-agent"
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
    }