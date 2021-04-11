pipeline {
        agent any
	tools {
       		maven "apache-maven-3.0.1"
              }
        stages {
                stage('Build') {
                        steps { 
                                sh 'mvn clean'
                        }
                }
                stage('Test') {
                        steps { 
                                sh 'mvn test'
                        }
                }
                stage('Compile') {
                        steps { 
                                sh 'mvn compile'
                        }
}
                stage('Package') {
                        steps { 
                                sh 'mvn package'
                        }
                }
		stage('Build Docker Image') {
                        steps {
                                sh 'docker build -t addressbook:latest .'
                        }
                }
		stage('Tag Docker Image') {
                        steps {
                        }
				script { 
					def version = readMavenPom().getVersion()
					sh 'docker tag addressbook:latest vasistaops/addressbook:${version}'
                		}
		stage('Push Docker Image') {
                        steps {
                                withCredentials([usernamePassword(credentialsId: 'DOCKER_CREDS', passwordVariable: 'DOCKER_HUB_PWD', usernameVariable: 'DOCKER_HUB_USER')]) {
                                sh 'docker login -u $DOCKER_HUB_USER -p $DOCKER_HUB_PWD'
                                }
                                sh 'docker push vasistaops/addressbook:pom.version'
                        }
                }
        }
}
