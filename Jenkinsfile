pipeline { 
    agent any  
    stages { 
        stage('Build') { 
            steps { 
                withDockerContainer(image: 'maven-selenium-chrome') {
                    sh """
                        mvn install
                    """
                }
            }
        }
    }
}