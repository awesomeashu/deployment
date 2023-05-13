pipeline {
    agent any

    environment {
        IMAGE_NAME=''
    }

    stages {
        stage('Clone configuration repository') {
            steps {
                checkout([$class: 'GitSCM', 
                          branches: [[name: '*/master']], 
                          userRemoteConfigs: [[url: 'https://github.com/awesomeashu/deployment.git']]])
            }
        }

        stage('Build and push frontend image') {
            steps {
                checkout([$class: 'GitSCM', 
                          branches: [[name: '*/master']], 
                          userRemoteConfigs: [[url: 'https://github.com/awesomeashu/enquiryclient-master.git']]])

                script {
                    IMAGE_NAME=docker.build "ashutosh024/spe_frontend"
                    docker.withRegistry('','docker-hub'){
                      IMAGE_NAME.push()
                    }
                }
            }
        }

        stage('Build and push backend image') {
            steps {
                checkout([$class: 'GitSCM', 
                          branches: [[name: '*/master']], 
                          userRemoteConfigs: [[url: 'https://github.com/awesomeashu/EnquiryServer-master.git']]])

                script {
                    IMAGE_NAME=docker.build "ashutosh024/spe_backend"
                    docker.withRegistry('','docker-hub'){
                      IMAGE_NAME.push()
                    }
                }
            }
        }

        stage('Deploy application using Ansible') {
            steps {
                sh "ansible-playbook /home/ashutosh/Desktop/deployment/ansible/deploy.yml -i /home/ashutosh/Desktop/deployment/ansible/inventory"
                //ansiblePlaybook becomeUser: null, colorized: true, disableHostKeyChecking: true, installation: 'Ansible', inventory: 'ansible/inventory',
                 //playbook: 'ansible/deploy.yml', sudoUser: null
            }
        } 
    }
}