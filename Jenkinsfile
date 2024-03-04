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
        stage('Dependencies') {
            steps {
                sh 'npm install -g @angular/cli && npm install'
                sh 'npm install -g echo-cli'
            }
        }
        stage('Build project') {
            steps {
                sh 'ng build'
            }
        }
        stage('Start project') {
            steps {
                sh 'npm start &'
            }
        }
		stage('Retrieve Git info') {
            steps {
                script {
                    env.MY_GIT_AUTHOR = sh(script: "git show -s --pretty=%an", returnStdout: true).trim()

                    echo "Git Commit: ${env.GIT_COMMIT}"
                    echo "Git Author: ${env.MY_GIT_AUTHOR}"
					echo "Git Branch: ${env.GIT_BRANCH}"
                }
            }
        }
        stage('Accessibility analysis...') {
        	steps {
                script {
					dir(params.PATH_TO_DIR) {
						def findCommand = "find . -type f -name '*component.ts' -exec readlink -f {} \\;"
	                    
	                    def tsFiles = sh(script: findCommand, returnStdout: true).trim().split('\n')
	                    
	                    println "Analyzing the following components:"
	                    tsFiles.each { sh(script: "echo-cli ${it + " " + params.PATH_TO_ROUTE_FILE + "/" + params.ROUTE_FILE_NAME}", returnStdout: true).trim() }
					}
                }
            }
        }
		stage('API semaphore') {
            steps {
                script {
                    //def response = httpRequest(url: params.ENDPOINT_URL + "?commit=${env.GIT_COMMIT}", httpMode: 'GET')
					def response = httpRequest(url: params.ENDPOINT_URL, httpMode: 'GET')
                    if (response.status == 200) {
                        echo "Endpoint response: ${response.status}"
                    } else {
                        error "Check accessibilità fallito: ${response.status}"
                    }
                }
            }
        }
    }

	post {
        always {
            script {
                sh 'pkill -f "ng serve"'
            }
        }
    }
}
