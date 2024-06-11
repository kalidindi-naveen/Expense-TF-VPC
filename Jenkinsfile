pipeline {
    agent {
        label 'naveen-2'
    }
    options {
        timeout(time: 30, unit: 'MINUTES')
        disableConcurrentBuilds()
        ansiColor('xterm')
    }
    parameters {
        choice(name: 'action', choices: ['Apply', 'Destroy'], description: 'Pick Choice')
    }
    stages {
        stage('Init') {
            steps {
              sh """
                terraform init -reconfigure
              """
            }
        }
        stage('Plan') {
            when {
                expression{
                    params.action == 'Apply'
                }
            }
            steps {
                sh """
                terraform plan
                """
            }
        }
        stage('Deploy') {
            when {
                expression{
                    params.action == 'Apply'
                }
            }
            input {
                message "Should we continue?"
                ok "Yes, we should."
            }
            steps {
                sh """
                terraform apply -auto-approve
                """
            }
        }

        stage('Destroy') {
            when {
                expression{
                    params.action == 'Destroy'
                }
            }
            steps {
                sh """
                terraform destroy -auto-approve
                """
            }
        }
    }
    post { 
        always { 
            echo 'Doing Cleanup........!'
            deleteDir()
        }
        success { 
            echo 'Pipeline is success........!'
        }
        failure { 
            echo 'Pipeline is failed..........!'
        }
    }
}