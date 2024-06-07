pipeline {
    agent any
    stages{
        stage('Clone FinanceMeProject'){
            steps{
                git url:'https://github.com/akanksha19012002/financeme.git'
            }
        }

        stage('Packaging the code'){
            steps{
                sh 'mvn clean package'
            }
        }
        stage('Building Docker image'){
            steps{
                script{
                    sh 'docker build -t financebanking .'
                    sh 'docker images'
                }
            }
        }

        stage('Containerization and Deployment'){
            steps{
                sh 'docker run -itd --name CBScontainer -p 8082:8081 financebanking'
            }
        }

        stage('Image tag and push to DockerHub'){
            steps{
                script{
                        sh 'docker tag financebanking akankshapande19/bankingimage:v1'
                        sh 'sudo docker push akankshapande19/bankingimage:v1'
                }

            }
        }


   }
}
