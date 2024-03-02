pipeline {
	
    agent any

    tools { nodejs "nodejs" }

    parameters {
    	string(name: 'PATH_TO_DIR', defaultValue: 'src/app', description: '')
        string(name: 'PATH_TO_ROUTE_FILE', defaultValue: 'src/app', description: '')
        string(name: 'ROUTE_FILE_NAME', defaultValue: 'app-routing.module.ts', description: '')
		string(name: 'ENDPOINT_URL', defaultValue: 'https://jsonplaceholder.typicode.com/todos/1', description: '')
    }
	
    stages {
		stage('Sonarqube') {
		    environment {
		        scannerHome = tool 'SonarQubeScanner'
		    }
		    steps {
		    	checkout scm
		        withSonarQubeEnv('SonarQubeServer') {
		            sh "${scannerHome}/bin/sonar-scanner"
		        }
		        timeout(time: 10, unit: 'MINUTES') {
		            waitForQualityGate abortPipeline: true
		        }
		    }
		}
        /*stage('Dependencies') {
            steps {
                sh 'npm install -g @angular/cli && npm install'
                sh 'npm install -g echo-cli'
            }
        }
        stage('Build') {
            steps {
                sh 'ng build'
            }
        }
        stage('Start') {
            steps {
                sh 'npm start &'
            }
        }
        stage('Routes retrieve') {
            steps {
                script {
                    dir(params.PATH_TO_ROUTE_FILE) {

                        def routeFileContent = readFile(params.ROUTE_FILE_NAME).trim()
                        
                        def paths = (routeFileContent =~ $/[\s]*path[\s]*:[\s]*'([^']*)'/$)
                        
                        def pathArray = []
                        
                        paths.each { match ->
                            pathArray.add(match[1])
                        }
                        
                        env.ROUTES = pathArray.join('\n')
                    }
                }
            }
        }
		stage('.html files retrieve') {
			steps {
				script {
					dir(params.PATH_TO_DIR) {
						def findCommand = "find . -type f -name '*component.html' -exec readlink -f {} \\;"
	                    
	                    def htmlFiles = sh(script: findCommand, returnStdout: true).trim().split('\n')
	                    
	                    println "Percorsi dei file HTML trovati:"
	                    htmlFiles.each { println "file://" + it }
					}
				}
			}
		}
		stage('Git info') {
            steps {
                script {
                    def gitCommit = env.GIT_COMMIT
                    def gitAuthor = sh(script: "git show -s --pretty=%an", returnStdout: true).trim()
					def gitBranch = env.GIT_BRANCH

                    echo "Git Commit: ${gitCommit}"
                    echo "Git Author: ${gitAuthor}"
					echo "Git Branch: ${gitBranch}"
                }
            }
        }
        stage('Npm calls for each route') {
            steps {
                script {
                    def routesArray = env.ROUTES.tokenize('\n')
                    routesArray.each { route ->
                        def output = sh(script: "echo-cli ${route}", returnStdout: true).trim()
                        println "Route: ${route}, Output: ${output}"
                    }
                }
            }
        }
		stage('API semaphore') {
            steps {
                script {
                    def response = httpRequest(url: params.ENDPOINT_URL, httpMode: 'GET')
					//def response = httpRequest(url: params.ENDPOINT_URL + "?commit=${env.GIT_COMMIT}", httpMode: 'GET')
                    if (response.status == 200) {
                        echo "Endpoint response: ${response.status}"
                    } else {
                        error "Check accessibilità fallito: ${response.status}"
                    }
                }
            }
        }*/
    }
}
