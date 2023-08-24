// pipeline {
//   agent any
//   environment {
// 	registryName = "materialdashdemo"
// 	registryUrl = "materialdashdemo.azurecr.io"
// 	registryCredential = "ACR"
// 	dockerImage = ''
//   }
//   tools {nodejs "node"}
//   post { 
//         always { 
//             cleanWs()
//         }
//     }
 
//   stages {
//     stage('Production Build') {
//       steps {
//         sh 'npm install'
// 		    sh 'npm run build'
//       }
//     }
// //Sonarqube
//     stage('SonarQube analysis') {
//       steps {
//         script {
//           def scannerHome = tool 'sonarqubescanner';
//           withSonarQubeEnv('sonarqubeserver') {
//             sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=mdreactapp -Dsonar.projectName=mdreactapp"
//           }
//         }
//       }
//     }
// //Create docker image
// 	  stage('Create Docker Image') {
// 	    steps {
// 		    script {
// //Build docker image
// 		  	  dockerImage = docker.build registryName

// 		    }
// 	    }
// 	  }
// // Uploading Docker images into ACR
//     stage('Upload Image to ACR') {
//       steps{   
//         script {
//           docker.withRegistry( "http://${registryUrl}", registryCredential ) {
//           dockerImage.push("${env.BUILD_NUMBER}")
//           }
//         }
//       }
//     }
// //Deploy ACR image to AKS
//     stage ('AKS Deploy') {
//       steps {
//         script {
//           withKubeConfig([credentialsId: 'K8S', serverUrl: '']) {
//           sh ('kubectl apply -f deployment.yaml')
//           // sh 'kubectl set image materialdashdemo.azurecr.io/materialdashdemo=${BUILD_NUMBER}
//               }
//           }
//         }
//      }
//   }
// }




// pipeline {
//     agent any
//     tools {nodejs "NodeJs"}

//     environment {
//         AWS_DEFAULT_REGION = 'us-east-1'
//         S3_BUCKET = 'demo-pro-java'
//     }

//     stages {
//         stage('Git Checkout') {
//             steps {
//                 script {
//                     git(
//                         credentialsId: 'karthik_GitHub_Credentials',
//                         url: 'https://github.com/BG-DevSecOps/md-test.git',
//                         branch: 'main'
//                     )
//                 }
//             }  
//         }

//         stage('NPM Version Check') {
//             steps {
//                 sh 'npm -v'
//             }
//         }

//         stage('Build') {
//             steps {
//                 sh 'npm install'
//                 sh 'npm run build'
//             }
//         }

//         stage('Deploy to S3') {
//             steps {
//                 withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'AWS_Credentials', accessKeyVariable: 'AWS_ACCESS_KEY_ID', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
//                     sh 'whoami'
//                     sh "/var/lib/jenkins/awscli-env/bin/aws s3 cp build/ s3://${S3_BUCKET}/ --recursive --region ${AWS_DEFAULT_REGION}"
//                 }
//             }
//         }
//     }
    
//     post {
//         always {
//             sh 'rm -rf node_modules build'
//         }
//     }
// }

// ///////////////////////////////////////////pipeline///////////////////////////////////////


// pipeline {
//     agent any
//     tools {nodejs "NodeJs"}

//     environment {
//         AWS_DEFAULT_REGION = 'us-east-1'
//         DEV_S3_BUCKET = 'demo-pro-java'
//         PROD_S3_BUCKET = 'demo-pro-java-prod'
//         BRANCH_NAME = sh(script: 'echo $GIT_BRANCH | cut -d/ -f2', returnStdout: true).trim()
//     }

//     stages {
//         stage('NPM Version Check') {
//             steps {
//                 sh 'npm -v'
//             }
//         }

//         stage('Build') {
//             steps {
//                 sh 'npm install'
//                 sh 'npm run build'
//             }
//         }

//         stage('Debug Info') {
//             steps {
//                 sh 'echo BRANCH_NAME: $BRANCH_NAME'
//             }
//         }

//         stage('Deploy to S3') {
//             steps {
//                 script {
//                     if (BRANCH_NAME == 'main') {
//                         sh "/var/lib/jenkins/awscli-env/bin/aws s3 cp build/ s3://${PROD_S3_BUCKET}/ --recursive --region ${AWS_DEFAULT_REGION}"
//                     } else if (BRANCH_NAME == 'dev') {
//                         sh "/var/lib/jenkins/awscli-env/bin/aws s3 cp build/ s3://${DEV_S3_BUCKET}/ --recursive --region ${AWS_DEFAULT_REGION}"
//                     } else {
//                         error "Unsupported branch: ${BRANCH_NAME}"
//                     }
//                 }
//             }
//         }
//     }
    
//     post {
//         always {
//             sh 'rm -rf node_modules build'
//         }
//     }
// }

///////////////////////////////////////////////////////////////////////////////////////////////////

pipeline {
    agent any
    tools {nodejs "NodeJs"}

    environment {
        AWS_DEFAULT_REGION = 'us-east-1'
        DEV_S3_BUCKET = 'demo-pro-java'
        PROD_S3_BUCKET = 'demo-pro-java-prod'
    }

    stages {
        stage('NPM Version Check') {
            steps {
                sh 'npm -v'
            }
        }

        stage('Build') {
            steps {
                sh 'npm install'
                sh 'npm run build'
            }
        }

        stage('Debug Info') {
            steps {
                sh 'echo BRANCH_NAME: $BRANCH_NAME'
            }
        }

        stage('Deploy to S3') {
            steps {
                script {
                    if (BRANCH_NAME == 'main') {
                        sh "/var/lib/jenkins/awscli-env/bin/aws s3 cp build/ s3://${PROD_S3_BUCKET}/ --recursive --region ${AWS_DEFAULT_REGION}"
                    } else if (BRANCH_NAME == 'dev') {
                        sh "/var/lib/jenkins/awscli-env/bin/aws s3 cp build/ s3://${DEV_S3_BUCKET}/ --recursive --region ${AWS_DEFAULT_REGION}"
                    } else {
                        error "Unsupported branch: ${BRANCH_NAME}"
                    }
                }
            }
        }
    }
    
    post {
        always {
            sh 'rm -rf node_modules build'
        }
    }
}
