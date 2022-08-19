pipeline {
    agent {
        label 'ansible_docker'
    }
    stages {
        stage('checkout') {
            steps {
                git 'https://github.com/vitalykay/example-playbook.git'
            }
        }
        stage('requirements') {
            steps {
                sh "ansible-galaxy role install -p roles -r requirements.yml java"
            }
        }
        stage('playbook') {
            steps {
                sh "ansible-playbook -i inventory/prod.yml site.yml"
            }
        }
    }
}
