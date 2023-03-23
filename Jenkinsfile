pipeline {
    
    agent {
        label 'docker-agent1'
    }

    environment {
        imageName = "mypython-flaskapp"
        tagName = "v1.0.0"
        profileDockerHub = "priyasivath"
        scannerHome = tool "SonarScanner-Linux"
    }
    
    stages {

        stage('Git Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/PriyaSIVATH/python-flask-sample-app.git']])
            }
        }

        stage('Image Build') {
            steps {
                // One or more steps need to be included within the steps block.
                sh "docker build -t ${imageName+':'+tagName} ."
            }
        }

        stage('SonarQube Code Analysis') {
            steps {
                // One or more steps need to be included within the steps block.
                withSonarQubeEnv(installationName: 'sonarqube1') {
                    // some block
                   sh "${scannerHome}/bin/sonar-scanner" 
                }
            }
        }

        stage("Quality Gate Status Check"){
            steps {
                script {
                    // timeout(time: 1, unit: 'HOURS') {
                    timeout(time: 2, unit: 'MINUTES') {  
                        def qg = waitForQualityGate()
                        if (qg.status != 'OK') {
                            error "Pipeline aborted due to quality gate failure: ${qg.status}"
                        }    
                    }  
                }   
            }
        }

        
        // stage('Sequential Stage - Docker Hub') {
        //     stages {
        //         // One or more stages need to be included within the stages block.
        //         stage('Docker Image Build') {
    	//             // agent any
        //             steps {
      	//                 // sh 'docker build -t python-flask-app:v1.0.0 .'
        //                 sh "docker build -t ${profileDockerHub+'/'+imageName+':'+tagName} ."
        //             }
        //         }
        //         stage('Docker Image Push') {
        //             steps {
        //                 // One or more steps need to be included within the steps block.
        //                 // This step should not normally be used in your script. Consult the inline help for details.
        //                 withDockerRegistry(credentialsId: 'DockerHub-Credentials', url: '') {
        //                 // some block
        //                     sh "docker push ${profileDockerHub+'/'+imageName+':'+tagName}"
        //                 }
        //             }
        //         }
        //     }
        // }
        
        

        

    }
}
