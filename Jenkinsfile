pipeline {
    agent any

    stages {

        stage('PULL') {
            steps {
                git 'https://github.com/HarshCraft/cdec-b21.git'
            }
        }
        stage('INFRA - EKS Creation') {
            steps {
                script {
                    // Load your infra pipeline file
                    def infra = load 'infra-pipeline.groovy'
                    infra.runInfra()
                }
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
                              -Dsonar.projectKey=sonarp2 \
                              -Dsonar.projectName=sonarp2
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
                dir('backend')
                     {
                        sh '''
                        aws s3 cp target/student-registration-backend-0.0.1-SNAPSHOT.jar s3://diamond-head/student-artifact.jar
                        '''
                    }
                }
            }
	}
}
