pipeline {
  agent any

  stages {
    stage ('Clone project from git') {
      steps {
        git 'https://github.com/Galend-E/DevOps_HW_final_v2.git'
        sh 'sed -i s/docker_password/$pass/g build.yml'
      }
    }
    stage ('Create 2 instances') {
      steps {
        sh 'terraform init'
        sh 'terraform plan -out instances.tfplan'
        sh 'terraform apply -auto-approve'
      }
    }
    stage ('Build and push boxfuse image') {
      steps {
        ansiblePlaybook become: true, credentialsId: 'b966c5be-f66e-4ae4-a58e-9fe403d4ef92', disableHostKeyChecking: true, installation: 'ansible_2.9.13', inventory: 'hosts', limit: 'build', playbook: 'build.yml'
      }
    }
    stage ('Pull and run boxfuse image') {
      steps {
        ansiblePlaybook become: true, credentialsId: 'b966c5be-f66e-4ae4-a58e-9fe403d4ef92', disableHostKeyChecking: true, installation: 'ansible_2.9.13', inventory: 'hosts', limit: 'web', playbook: 'web.yml'
      }
    }
  }
}