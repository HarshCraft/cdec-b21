pipeline {
    agent none   // No global agent

    stages {

        stage('PULL') {
            agent {
                docker {
                    image 'alpine/git'
                    label 'slave1a'
                }
            }
            steps {
                git 'https://github.com/HarshCraft/cdec-b21.git'
            }
        }

        stage('BUILD') {
            agent {
                docker {
                    image 'maven:3.9.6-eclipse-temurin-17'
                    label 'slave1a'
                }
            }
            steps {
                dir('backend') {
                    sh 'mvn clean package -DskipTests'
                }
            }
        }

        stage('TEST') {
            agent {
                docker {
                    image 'maven:3.9.6-eclipse-temurin-17'
                    label 'slave1a'
                }
            }
            steps {
                echo "Running Tests"
            }
        }

        stage('DEPLOY') {
            agent {
                docker {
                    image 'alpine'
                    label 'slave1a'
                }
            }
            steps {
                echo "Deploying Application..."
            }
        }
    }
}
