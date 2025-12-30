pipeline {
    agent any {
	label 'slave1a'
	}

    stages {

        stage('PULL') {
            steps {
                git 'https://github.com/HarshCraft/cdec-b21.git'
            }
        }

        stage('BUILD') {
            steps {
                dir('backend') {
                    sh '''
                    mvn clean package -DskipTests
                    '''
                }
            }
        }

        stage('TEST') {
            steps {
                echo "TEST SUCCESS"
            }
        }

        stage('DEPLOY') {
            steps {
                echo "DEPLOY SUCCESS"
            }
        }
    }
}
