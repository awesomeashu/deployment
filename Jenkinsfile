pipeline {
    agent any

    environment {
        IMAGE_NAME=''
        KUBECONFIG='/home/aryan/Desktop/Placement/Spe_Major_deploy/auth.yml'
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
                sh " cd /home/aryan/Desktop/Placement/Spe_Major_deploy/ansible && ansible-playbook -i inventory deploy.yml"
            }
        }
    }
}