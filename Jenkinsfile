def secret = 'vm1'
def server = 'komarhidayat0@103.127.132.193'
def directory = '/home/komarhidayat0/wayshub/wayshub-frontend'
def branch  = 'master'
def namebuild = 'wayshub-frontend'
def tag = 'production'

pipeline {
    agent any
    stages{
	stage ('pull new code') {
	   steps{
		sshagent([secret]){
		   sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
		   cd ${directory}
		   git pull origin ${branch}
		   echo "Pulling new code telah selesai"
                   exit
                   EOF"""
	       }
	    }
         }
          
        stage ('build the code') {
           steps{
               sshagent([secret]){
                  sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
                  cd ${directory}
                  docker buildx build -t ${namebuild}:${tag} .
                  echo "Build code telah selesai"
                  exit
		  EOF""" 
  	        }	
             }
          } 

        stage ('deploy') {
           steps {
               sshagent([secret]){
                  sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
                  cd ${directory}
                  docker compose down
                  docker compose up -d
                  echo "deployment telah selesai"
                  exit
                  EOF"""
                }             
            }
        }
    }
}
