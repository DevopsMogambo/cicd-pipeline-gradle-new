pipeline {
    agent any
    stages {
        stage('CodeCheckOut') {
            steps {
                script {
                    checkout scm
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
						script {
							sh "npm install"
							}
					}
				}
				stage('npm Start') {
					steps {
						script {
							try {
									sh "npm start"
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
				}

		}   

