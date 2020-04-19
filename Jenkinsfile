pipeline {
  agent any
     stages {
      stage('SCM Checkout') {
       steps {
          checkout scm
         }
        }
      stage('Build') {
       steps {
          echo "Cleaning and packaging.."
          sh 'mvn clean package'
         }
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
           step([$class: 'KubernetesEngineBuilder', projectID: "wired-tea-264519", clusterName: "k8-cluster", location: "us-central1-c", credentialsId: "rajeshbhan", verifyDeployments: true])
           echo "Deployment Finished.."
          }
        }
      }
   }
