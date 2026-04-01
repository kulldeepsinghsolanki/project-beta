pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Cloning repository...'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Building project with Maven...'
                sh '/opt/maven/bin/mvn clean install -DskipTests'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                sh '/opt/maven/bin/mvn test'
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
                echo 'Archiving artifact...'
                archiveArtifacts artifacts: 'target/*.war', fingerprint: true
            }
        }

        stage('Run Ansible') {
            steps {
                echo 'Executing Ansible playbook...'
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
            echo 'Build and Ansible execution SUCCESS ✅'
        }
        failure {
            echo 'Build FAILED ❌'
        }
        always {
            echo 'Pipeline finished'
        }
    }
}