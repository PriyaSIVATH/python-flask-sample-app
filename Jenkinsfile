pipeline {
    
    agent {
        label 'docker-agent1'
    }

    environment {
        imageName = "mypython-flaskapp"
        tagName = "v1.0.0"
        profileDockerHub = "priyasivath"
    }
    
    stages {
        stage('Git-Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/PriyaSIVATH/python-flask-sample-app.git']])
            }
        }
        
        stage('Sequential Stage - Docker Hub') {
            stages {
                // One or more stages need to be included within the stages block.
                stage('Docker Image Build') {
    	            // agent any
                    steps {
      	                // sh 'docker build -t python-flask-app:v1.0.0 .'
                        sh "docker build -t ${profileDockerHub+'/'+imageName+':'+tagName} ."
                    }
                }
            }
        }
        
        

        

    }
}
