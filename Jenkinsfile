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
        sh 'ANSIBLE_HOST_KEY_CHECKING=False ansible-playbook -u ubuntu --private-key ~/.ssh/ansible.pem -i hosts build.yml'
      }
    }
    stage ('Pull and run boxfuse image') {
      steps {
        sh 'ANSIBLE_HOST_KEY_CHECKING=False ansible-playbook -u ubuntu --private-key ~/.ssh/ansible.pem -i hosts web.yml'
      }
    }
  }
}