pipeline {
  agent any
     stages {
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
           step([$class: 'KubernetesEngineBuilder', projectID: "wired-tea-264519", clusterName: "k8-cluster", location: "us-central1-c", credentialsId: "rajeshk8test", verifyDeployments: true])
           echo "Deployment Finished.."
          }
        }
      }
   }
