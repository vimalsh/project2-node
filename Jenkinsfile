pipeline {
    agent { label 'Jenkins_Slave2' }

    environment {
		DOCKERHUB_CREDENTIALS=credentials('dockerhub-vimal')
	}

    stages {
        stage('Git Clone') {
            steps {
                git url: 'https://github.com/vimalsh/project1-php.git', branch: 'main'
            }
        }
        stage('Install Docker') {
            agent any
            steps{
                ansiblePlaybook credentialsId: 'ansible-slave', installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: '/etc/ansible/dockerplaybook.yml'
            }
        }
       stage('Build'){
            steps {
                sh 'docker build -t 08091993deadly/phpapp:latest .'
            }
        }
        stage('Login DockerHub') {
			steps {
				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
			}
		}
        stage('Run'){
            steps {
                sh 'docker run -d -p 80:80 08091993deadly/phpapp:latest'
            }
        }
        stage('Push') {
			steps {
				sh 'docker push 08091993deadly/phpapp:latest'
			}
		}
         stage('Logout DockerHub') {
			steps {
				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
			}
		}
    }
}