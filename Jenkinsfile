pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "maven3"
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Get some code from a GitHub repository
                git branch: params.BRANCHE , url: 'https://github.com/papebily/maven_with_bugs.git'
            }
        }
        stage('Checkout Code && Build Maven') {
            steps {
                script{
                    switch(params.BRANCHE){
                        case "main": sh "mvn -DskipTests clean package";break
                        case "stage": sh "mvn -DskipTests clean package";break
                    }
                    // Run Maven on a Unix agent.
                }
            }
        }
        stage('SonarQube Analysis') {
            steps {
                // Execute SonarQube analysis
                script {
                    def scannerHome = tool 'sonarscanner'
                    withSonarQubeEnv('sonarqubeserver') {
                        sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=vprofile \
                        -Dsonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest/"
                    }
                }
            }
        }  
    }
}
