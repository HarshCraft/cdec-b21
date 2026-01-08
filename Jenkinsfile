pipeline {
    agent any

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
                withSonarQubeEnv('sonar-token1') {
                    dir('backend') {
                        sh '''
                            mvn org.sonarsource.scanner.maven:sonar-maven-plugin:sonar \
                              -Dsonar.projectKey=sonarp \
                              -Dsonar.projectName=sonarp
                        '''
                    }
                }
            }
        }

stage('QUALITY GATE') {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage('DELIVERY') {
            steps {
                dir('backend') {
                    withCredentials([
                        usernamePassword(
                            credentialsId: 'aws-cred',
                            usernameVariable: 'AWS_ACCESS_KEY_ID',
                            passwordVariable: 'AWS_SECRET_ACCESS_KEY'
                        )
                    ]) {
                        sh '''
                        export AWS_DEFAULT_REGION=eu-west-1
                        aws s3 cp target/student-registration-backend-0.0.1-SNAPSHOT.jar \
                        s3://diamond-head/student-artifact.jar
                        '''
                    }
                }
            }
        }

	 stage('DEPLOY') {
            steps {
                echo "DEPLOY SUCCESS"
            }
        }

    }
}
