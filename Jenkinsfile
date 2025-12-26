pipeline {
    agent any

    parameters {
        choice(
            name: 'ACTION',
            choices: ['plan', 'apply'],
            description: 'Select the action to perform'
        )
        choice(name: 'BRANCH', choices: ['main', 'develop', 'feature/example'], description: 'Select branch to build')
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scmGit(branches: [[name: "*/${params.BRANCH}"]], extensions: [], userRemoteConfigs: [[url: 'https://github.com/priteedivekar/Terraform-Automation.git']])
                echo "Checked out branch ${params.BRANCH}"
            }
        }
    
        stage ("terraform init") {
            steps {
                sh ("terraform init -reconfigure") 
            }
        }

        stage ("Action") {
            steps {
                script {
                    switch (params.ACTION) {
                        case 'plan':
                            echo 'Executing Plan...'
                            sh "terraform plan"
                            break
                        case 'apply':
                            echo 'Executing Apply...'
                            sh "terraform apply --auto-approve"
                            break
                        default:
                            error 'Unknown action'
                    }
                }
            }
        }
    }
}
