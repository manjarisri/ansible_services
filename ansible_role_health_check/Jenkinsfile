pipeline {
    agent any
    stages {
        stage('gitcheckout') {
          steps{                
	        git branch: 'main', url: 'https://github.com/manjarisri/ansible_services.git'  
          }  
        }
	    
        stage('ansible script') {
          steps{
	        sh 'ansible all -i inven.inv -m ping'
	      	sh 'ansible-playbook -i hosts/inven playbook.yaml'	  
          }  
        }
    }
}
