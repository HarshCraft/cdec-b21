pipeline {
	agent any 
	
	stages {

	stage('PULL') {
		steps{
			git 'http://github.com/HarshCraft/cdec-b21.git'
		}
	}
	stage('BULD') {
		steps{
		dir('backend') {
			sh'''
			mvn clean package -DskipTests
			'''
			}
		}
	}
	stage('TEST') {
		steps {
			echo "Test success"
		}
	}
	stage('Deploy') {
		steps {
			echo "Deploy success"
		}
	}
	}
} 
	
