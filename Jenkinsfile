pipeline {
    agent any
    stages{
        stage('Clone FinanceMeProject'){
            steps{
                git url:'https://github.com/prad-uction/FinanceMeProject.git'
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
                sh 'docker run -itd --name CBS -p 8082:8081 financebanking'
            }
        }

        stage('Image tag and push to DockerHub'){
            steps{
                script{
                        sh 'docker tag financebanking pradocks/bankingimage:v1'
                        sh 'sudo docker push pradocks/bankingimage:v1'
                }

            }
        }

     stage('Deploy on Ansible') {
            steps {
              ansiblePlaybook become: true, credentialsId: 'dockerhub-pwd', disableHostKeyChecking: true, installation: 'Ansible', inventory: '/etc/ansible/hosts', playbook: 'ansible-playbook.yml', vaultTmpPath: ''
            }
        }
    }
   }
}
