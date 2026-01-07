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
        stage('DEPLOY') {
            steps {
                echo "DEPLOY SUCCESS"
            }
        }
    }
}
