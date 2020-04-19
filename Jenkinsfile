pipeline {
  agent any
      environment {
        
          PROJECT_ID = 'wired-tea-264519'
          CLUSTER_NAME = 'k8-cluster'
          LOCATION = 'us-central1-c'
          CREDENTIALS_ID = 'rajeshk8test'
     }
     
     stages {
          stage('SCM Checkout') {
              steps {
                  sh 'checkout scm'
                 }
               }
               
          stage('Build') {
              steps {
                  echo "Cleaning and packaging.."
                  sh 'mvn clean package'
                }
                
          stage('Test') {
              steps {
                  echo "Testing.."
                  sh 'mvn test'
                 }
               }
               
          stage('Build Docker Image') {
              steps {
                  script {
                      myimage = docker.build("rajeshbhan/tomcat01:${env.BUILD_ID}")
                    }
                  }
                }
                
          stage('Push Docker Image') {
              steps {
                    script {
                        docker.withRegistry('https://registry.hub.docker.com', 'docker') {
                            myimage.push("${env.BUILD_ID}")
                          }
                        }
                      }
                    }
             
          stage('Deploy to K8s') {
              steps {
                    echo "Deployment started.."
                    sh 'ls -ltr'
                    sh 'pwd'
                    sh "sed -i 's/tagversion/${env.BUILD_ID}/g' deployment.yaml"
                    step([$class: 'KubernetesEngineBuilder', projectID: env.PROJECT_ID, clusterName: env.CLUSTER_NAME, location: env.LOCATION, credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])
                    echo "Deployment Finished.."
                  }
                }
              }
            }
  }
