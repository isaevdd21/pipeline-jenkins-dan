# 04_02 SSH agent


create another ec2 ubuntu "jenkins-node"
create a keypair or use same one that you use for jenkins-server
connect to that server and use install.sh as we did it to "jenkins-server" which will install everything for us


in jenkins console go to Manage Jenkins > "Nodes" 
selet "New node" 
Node name: linux 
Remote root directory: /home/ubuntu
Labels: linux
Launch method: Only builds jobs with label expressions matching this node
Launch method: Launch agents via ssh
 - Host : dns name or IP address of the node
 - Credentials: choose and Jenkins and fill out as below to pop up window
   - Kind : ssh user with private key
    - ID : linux
    - Desciption: linux
    - username : ubuntu 
    - Private key: enter directly 
     - go to where your key is located and open it with cat and copy paste to a field
    Then on credentials choose ubuntu(linux)
Host key verification sttrategy use : Non veryfying verification strategy
save and go to Nodes page

in jenkins-server create pipeline "ssh-agent-linux" and run below code on it.

pipeline {
    agent { label 'linux' }
    
    stages {
        stage('Test') {
            steps {
                sh 'echo "Hello GeekDevops"'
            }
        }
        stage('kernel_version') {
            steps {
                dir("${env.WORKSPACE}/ssh-agent-linux"){
                    sh 'uname -r'
                }
            }
        }
        stage('Build-OS') {
            steps {
                dir("${env.WORKSPACE}/ssh-agent-linux"){
                    sh 'cat /etc/*-release'
                }
            }
        }
     
            
        }
    }