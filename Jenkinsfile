pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Cloning repo...'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Building with Maven...'
                sh 'mvn clean install -DskipTests'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'mvn test'
            }
        }

        stage('Verify Artifact') {
            steps {
                sh 'ls -l target/'
            }
        }

        stage('Ansible') {
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