pipeline {
	
    agent any

    tools { nodejs "nodejs" }
	
    stages {
        stage('Installazione dipendenze') {
            steps {
                // Esegui l'installazione delle dipendenze necessarie per Angular
                sh 'npm install -g @angular/cli && npm install'
                // Installa npm echo globalmente per poterlo utilizzare in tutta la pipeline
                sh 'npm install -g npm-echo'
            }
        }
        stage('Build del progetto') {
            steps {
                // Esegui la build del progetto Angular
                sh 'ng build --prod'
            }
        }
        stage('Avvio del progetto') {
            steps {
                // Avvia il progetto Angular
                sh 'npm start &'
            }
        }
        stage('Raccolta delle routes') {
            steps {
                // Esegui la raccolta delle routes dal file delle routes di Angular
                script {
                    // Definisci il percorso del file delle route
					def routeFilePath = '/src/app/app-routing.module.ts'
					
					// Leggi il contenuto del file delle route
					def routeFileContent = readFile(routeFilePath).trim()
					
					// Trova tutte le occorrenze della stringa 'path:'
					def paths = (routeFileContent =~ /'path': '([^']+)'/)
					
					// Crea un array per memorizzare i valori delle chiavi 'path'
					def pathArray = []
					
					// Estrai e aggiungi i valori delle chiavi 'path' all'array
					paths.each { match ->
						pathArray.add(match[1])
					}
					
					// Assegna l'array dei valori delle route alla variabile d'ambiente ROUTES
					env.ROUTES = pathArray.join('\n')
                }
            }
        }
        stage('Esecuzione di npm echo per ogni route') {
            steps {
                script {
                    // Divide le routes raccolte in un array
                    def routesArray = env.ROUTES.tokenize('\n')
                    // Itera su ogni route
                    routesArray.each { route ->
                        // Esegui npm echo per ogni route
                        sh "npm-echo ${route}"
                    }
                }
            }
        }
    }
}
