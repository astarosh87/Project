pipeline {
    agent {
        label 'master'
    }
    stages {
        stage('Git pull repo') {
            steps {
            git credentialsId: 'git_ssh_jenkins_my_comp', url: 'git@github.com:astarosh87/Project.git'
            }
        }
            stage('Print current directory and list of files') {
            steps {
                sh """
                pwd
                ls -la
                """
            }
        }
        stage('Test YAML') {
            steps {
                sh """
                cd ansible
                ansible-lint init_setup.yaml
                """
            }
        }
        stage('Init Install docker_container') {
            steps {
                ansiblePlaybook(
                    inventory: './ansible/hosts.yaml',
                    playbook: './ansible/init_setup.yaml'
                )    
            }
        }
    }
post {
        success {
                slackSend (color: 'good', message: "GOOD: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
        }
            failure {
                slackSend (color: 'danger', message: "BAD: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
        }
    }
}
