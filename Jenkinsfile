pipeline{
    agent any
    environment{
        VERSION = "${env.BUILD_ID}"
    }
    stages{
        stage("sonar quality check"){
            agent{
                docker {
                    image 'openjdk:11'
                } 
            }
            steps{  
                script{
                    withSonarQubeEnv(credentialsId: 'sonar') {
                        sh 'chmod +x gradlew'
                        sh './gradlew sonarqube'
                    }
                }

            }
        }
        stage('docker build & docker push'){
            steps{
                script{
                    withCredentials([string(credentialsId: 'nexus-repo', variable: 'nexus-pass')]) {
                             sh '''
                               docker build -t 3.85.63.183:8081/springapp:${VERSION} .
                               docker login -u admin -p $nexus-pass 3.85.63.183:8083
                               docker push 3.85.63.183:8081/springapp:${VERSION}
                               docker rmi 3.85.63.183:8081/springapp:${VERSION}
                    
                            '''
                   }

                }
            }

        }
    }
}