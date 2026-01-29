pipeline {
    agent any
    stages{
        stage('git cloned'){
            steps{
                git url:'https://github.com/Manjunath-Proj/php-project.git', branch: "master"
              
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t dewdropsmk/php-project:v1 .'
                    sh 'docker images'
                }
            }
        }
          stage('Docker login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-pwd', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                    sh 'docker push dewdropsmk/php-project:v1'
                }
            }
        }
    stage('Deploy') {
            steps {
               script {
                   def dockerrm = 'sudo docker rm -f php-project-con || true'
                    def dockerCmd = 'sudo docker run -itd --name php-project-con -p 8089:80 dewdropsmk/php-project:v1'
                    sshagent(['php-project']) {
                        //chnage the private ip in below code
                        // sh "docker run -itd --name My-first-containe2111 -p 8083:80 akshu20791/2febimg:v1"
                         sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.28.108 ${dockerrm}"
                         sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.28.108 ${dockerCmd}"
                    }
                }
            }
        }   
    }
}
