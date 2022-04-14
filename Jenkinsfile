pipeline { 
    agent any  
    stages { 
        stage('Build') { 
            steps { 
                withDockerContainer(image: 'maven') {
                    sh """
                        mvn install
                    """
                }
            }
        }
    }
}