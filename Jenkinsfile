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
         stage('Docker login') {
            steps {
                  withCredentials([usernamePassword(credentialsId: 'dockerhub-pwd', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                     sh "echo $PASS | docker login -u $USER --password-stdin"
                     sh 'sudo docker push akankshapande19/bankingimage:v1'
                }
            }

        stage('Containerization and Deployment'){
            steps{
                sh 'docker run -itd --name CBScontainer -p 8082:8081 financebanking'
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
