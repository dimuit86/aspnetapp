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
				git 'https://github.com/dimuit86/aspnetapp.git'
			}
		}

		// stage('Code Quality Check via SonarQube') {
		// 	steps {
		// 		script {
		// 		def scannerHome = tool 'SonarScannerMSBuild';
		// 			withSonarQubeEnv("SonarqubeServer") {
		// 			sh "dotnet ${scannerHome}/SonarScanner.MSBuild.dll begin /k:\"aspnetapp\""
		// 			sh "dotnet build"
		// 			sh "dotnet ${scannerHome}/SonarScanner.MSBuild.dll \
		// 				-Dsonar.projectKey=aspnetapp \
		// 				-Dsonar.sources=. \
		// 				-Dsonar.css.node=. \
		// 				-Dsonar.host.url=http://viqsonar.eastus.cloudapp.azure.com:9000/ \
		// 				-Dsonar.login=squ_3ef641a03e8177d7a0ef638f69fafc2414d15a59"
		// 			}

		// 		}
		// 	}
		// }

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