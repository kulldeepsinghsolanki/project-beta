pipeline {
    agent any

    tools {
        maven 'MAVEN'   // Make sure Maven is configured in Jenkins
        jdk 'JDK'       // Configure JDK in Jenkins (prefer JDK 11/17)
    }

    environment {
        ARTIFACT = "target/demo.war"
    }

    stages {

        stage('Checkout Code') {
            steps {
                echo 'Cloning repository...'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Building project using Maven...'
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
                echo 'Checking generated artifact...'
                sh 'ls -l target/'
            }
        }

        stage('Archive Artifact') {
            steps {
                echo 'Archiving artifact in Jenkins...'
                archiveArtifacts artifacts: 'target/*.war', fingerprint: true
            }
        }

        stage('Send to Ansible') {
            steps {
                echo 'Sending artifact to Ansible...'
                sh '''
                ansible-playbook /home/jenkins/ansible/send-artifact.yml \
                -i /home/jenkins/ansible/hosts \
                --extra-vars "artifact_path=$WORKSPACE/target/demo.war"
                '''
            }
        }
    }

    post {
        success {
            echo 'Build and Ansible execution SUCCESS'
        }
        failure {
            echo 'Build FAILED'
        }
        always {
            echo 'Pipeline finished'
        }
    }
}