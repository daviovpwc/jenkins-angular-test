pipeline {
	
    agent any

    tools { nodejs "nodejs" }
	
    stages {
        stage('Dependencies') {
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

		    dir("src/app") {

			    def routeFileContent = readFile("app-routing.module.ts").trim()
				
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
    }
}
