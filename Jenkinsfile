pipeline {
    agent any
    stages{
        stage('Clone FinanceMeProject'){
            steps{
                git url:'https://github.com/akanksha19012002/financeme.git', branch: "master"
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

        stage('Docker login, Image tag and push to DockerHub'){
            steps{
                script{
                     withCredentials([usernamePassword(credentialsId: 'dockerhub-pwd', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                        sh "echo $PASS | docker login -u $USER --password-stdin"
                        sh 'docker tag financebanking akankshapande19/bankingimage:v1'
                        sh 'docker push akankshapande19/bankingimage:v1'
                }
                }
            }
        }

     stage('Deploy on Ansible') {
            steps {
              ansiblePlaybook become: true, credentialsId: 'Ansible', disableHostKeyChecking: true, installation: 'Ansible', inventory: '/etc/ansible/hosts', playbook: 'ansible-playbook.yml', vaultTmpPath: ''
            }
        }
    }
   }
