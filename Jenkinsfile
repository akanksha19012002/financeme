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
                    sh 'echo "Akanksha@2002" | sudo -S docker build -t financebanking .'
                    sh 'docker images'
                }
            }
        }

        stage('Docker login, Image tag and push to DockerHub'){
            steps{
                script{
                     withCredentials([usernamePassword(credentialsId: 'dockerhub-pwd', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                        sh "echo $PASS | docker login -u $USER --password-stdin"
                        sh 'docker tag financebanking akankshapande19/bankingimage:v1'
                        sh 'sudo docker push akankshapande19/bankingimage:v1'
                }
                }
            }
        }

     stage('Deploy on Ansible') {
            steps {
              ansiblePlaybook become: true, credentialsId: 'Ansible', installation: 'Ansible', inventory: '/etc/ansible/hosts', playbook: 'ansible-playbook.yml', vaultTmpPath: ''
            }
        }
    }
   }
