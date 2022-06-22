pipeline {
	environment {
		registry = "dimuit86/simple-node-js-react-app"
		registryCredential = 'dockerhub_id'
		dockerImage = ''
	}
	agent any
	stages {
		stage('Cloning our Git') {
			steps {
				git 'https://github.com/dimuit86/node-js-react-npm-app.git'
			}
		}
		stage('SonarQube Analysis') {
			def msbuildHome = tool 'Default MSBuild'
			def scannerHome = tool 'SonarScannerMSBuild'
			withSonarQubeEnv() {
			bat "\"${scannerHome}\\SonarScanner.MSBuild.exe\" begin /k:\"aspnetapp\""
			bat "\"${msbuildHome}\\MSBuild.exe\" /t:Rebuild"
			bat "\"${scannerHome}\\SonarScanner.MSBuild.exe\" end"
			}
		}
		stage('Building our image') {
			steps {
				script {
					dockerImage = docker.build registry + ":$BUILD_NUMBER"
				}
			}
		}
		stage('Deploy our image') {
			steps {
				script {
					docker.withRegistry('', registryCredential) {
						dockerImage.push()
					}
				}
			}
		}
		stage('Cleaning up') {
			steps {
				sh "docker rmi $registry:$BUILD_NUMBER"
			}
		}
	}
}