pipeline {
	
    agent any

    tools { nodejs "nodejs" }

    parameters {
        string(name: 'PATH_TO_ROUTE_FILE', defaultValue: 'src/app', description: '')
        string(name: 'ROUTE_FILE_NAME', defaultValue: 'app-routing.module.ts', description: '')
    }
	
    stages {
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
        }*/
		stage('.html files retrieve') {
			steps {
				script {
					// Definisci il percorso iniziale
                    def initialPath = 'src'
                    
                    // Comando per trovare i file HTML
                    def findCommand = "find .${initialPath} -type f -name '*.png'-exec readlink -f {} \";
                    
                    // Esegui il comando per trovare i file HTML
                    def htmlFiles = sh(script: findCommand, returnStdout: true).trim().split('\n')
                    
                    // Stampa i percorsi trovati
                    println "Percorsi dei file HTML trovati:"
                    htmlFiles.each { println it }
				}
			}
		}
        /*stage('Npm calls for each route') {
            steps {
                script {
                    def routesArray = env.ROUTES.tokenize('\n')
                    routesArray.each { route ->
                        def output = sh(script: "echo-cli ${route}", returnStdout: true).trim()
                        println "Route: ${route}, Output: ${output}"
                    }
                }
            }
        }*/
    }
}
