pipeline {
    
    agent {
        label 'docker-agent1'
    }

    environment {
        imageName = "mypython-flaskapp"
        tagName = "v1.0.0"
        profileDockerHub = "priyasivath"
        scannerHome = tool "SonarScanner-Linux"
        registryNexus = "192.168.0.155:8085/"
        registryNexusCredentials = "nexus-repo-manager"
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
                sh "docker build -t ${registryNexus+imageName+':'+tagName} ."
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

        stage('Nexus Image Push ') {
            steps {
                script {
                    // //    // One or more steps need to be included within the steps block.
                    // //  // This step should not normally be used in your script. Consult the inline help for details.

                    // // withDockerRegistry(credentialsId: 'nexus-repo-manager', url: 'http://192.168.0.155:8085/') {
                    // withDockerRegistry(credentialsId: 'nexus-repo-manager', url: 'http://'+ registryNexus) {
                    // // withDockerRegistry(registryNexusCredentials, 'http://'+ registryNexus) {
                    //     //  // some block
                    //     // sh "docker push ${registry+'/'+imageName+':'+tagName}"
                    //     dockerImage.push("${tagName}") 
                    // }

                    /* docker.withRegistry( 'http://'+registryNexus, registryNexusCredentials) {
                        dockerImage.push("${tagName}") */

                    withDockerRegistry(credentialsId: 'nexus-repo-manager', url: 'http://'+ registryNexus) {
                      /*  //  Need to push availble image 
                        //  >> we have - mypython-flaskapp:v1.0.0  -- ${imageName+':'+tagName}
                        //  >> we need - 192.168.0.155:8085/mypython-flaskapp:v1.0.0  -- ${registryNexus+imageName+':'+tagName}     */

                        // 2 solutions 
                        // >> 1. can convert the exist image to needed image using -- DOCKER TAG
                        // >> 2. build image with regsitryUrl .. -- 192.168.0.155:8085/mypython-flaskapp
                    //    sh "docker push ${registryNexus+'/'+imageName+':'+tagName}" 

                        // sh "docker  tag  ${imageName+':'+tagName}  ${registryNexus+imageName+':'+tagName}"

                        sh "docker push ${registryNexus+imageName+':'+tagName}" 

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
