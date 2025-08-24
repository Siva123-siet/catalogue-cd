pipeline {
    // Pre-build
    agent {
        label 'AGENT-1'
    }
    environment { 
        appVersion = ''
        REGION = "us-east-1"
        ACC_ID = "097003440739"
        PROJECT = "roboshop"
        COMPONENT = "catalogue"
    }
    options {
        timeout(time: 30, unit: 'MINUTES') 
        disableConcurrentBuilds() 
    }
    parameters {
        string(name: 'appVersion', description: 'Image version of the application')
        choice(name: 'deploy_to', choices: ['dev', 'qa', 'prod'], description: 'Pick the Environment')
    }
    // Build 
    stages {
        stage('Deploy') {
            steps {
                script {
                  withAWS(credentials: 'aws-auth', region: 'us-east-1') {
                // Your AWS CLI commands or other AWS interactions go here
                sh """
                  aws eks update-kubeconfig --region $REGION --name "$PROJECT-${params.deploy_to}"
                  kubectl get nodes
               """
                }
                }
            }
        }
    }
    //post build
    post { 
        always { 
            echo 'I will always say Hello again!'
            deleteDir()
        }
        success { 
            echo 'Hello Success'
        }
        failure { 
            echo 'Hello failure'
        }
    }
}