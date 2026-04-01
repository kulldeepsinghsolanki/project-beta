pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Verify Artifact') {
            steps {
                sh 'ls -l target/'
            }
        }

        stage('Run Ansible') {
            steps {
                sh '''
                ansible-playbook /home/jenkins/ansible/send-artifact.yml \
                -i /home/jenkins/ansible/hosts \
                --extra-vars "artifact_path=$WORKSPACE/target/demo.war"
                '''
            }
        }
    }
}