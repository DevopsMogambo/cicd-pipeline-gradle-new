pipeline {
    agent any
    stages {
        stage('CodeCheckOut') {
            steps {
                script {
                    	checkout scm
			env.NODEJS_HOME = "${tool 'NodeJS'}"
    			// on linux / mac
   			 env.PATH="${env.NODEJS_HOME}/bin:${env.PATH}"
			sh "npm --version"
                    }
            }
        }
		stage('User Input') {
			steps {
			input ('Do you want to proceed?')
			}
		}
        stage('Build Customer app code') {
            steps {
                script {

                    try {
                        sh "./gradlew build"
                        currentBuild.result = 'SUCCESS'
                    } catch (Exception err) {
                        currentBuild.result = 'FAILURE'
                        sh "exit 1"
                    }
                    echo "RESULT: ${currentBuild.result}"
                }
            }
        }

		stage('npm SetUp'){
			parallel {
				stage ('npm Install'){
					steps {
						withNPM(npmrcConfig:'my-custom-nprc') {
            						echo "Performing npm build..."
            						sh 'npm install'
        				       		}
					}
				}
				stage('npm Version Check') {
					steps {
						script {
							try {
									withNPM(npmrcConfig:'my-custom-npmrc') {
            									echo "Performing npm version..."
            									sh 'npm --version'
        									}
									} catch (Exception err) {
										currentBuild.result = 'FAILURE'
										sh "exit 1"
									}
								echo "RESULT: ${currentBuild.result}"
									}
								}
							}
						}
					}
		stage ('Npm Start') {
			steps {
				script {
					try {
						withNPM(npmrcConfig:'my-custom-npmrc') {
            								echo "Performing npm Start..."
            								sh 'npm start'
        								}
								} catch (Exception err) {
									currentBuild.result = 'FAILURE'
									sh "exit 1"
								}
							echo "RESULT: ${currentBuild.result}"
								}
							}
						}
					}
			
				}
		
