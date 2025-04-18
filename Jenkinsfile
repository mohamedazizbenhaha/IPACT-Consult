pipeline{
    agent any



    stages {


        stage('Getting project from Git') {
            steps{
      			checkout([$class: 'GitSCM', branches: [[name: '*/main']],
			extensions: [],
			userRemoteConfigs: [[url: 'https://github.com/mohamedazizbenhaha/IPACT-Consult.git']]])
            }
        }


       stage('Cleaning the project') {
            steps{
                	sh "mvn -B -DskipTests clean  "
            }
        }



        stage('Artifact Construction') {
            steps{
                	sh "mvn -B -DskipTests package "
            }
        }



         stage('Unit Tests') {
            steps{
               		 sh "mvn test "
            }
        }



        stage('Code Quality Check via SonarQube') {
            steps{

             		sh "  mvn clean verify sonar:sonar -Dsonar.projectKey=Ipact -Dsonar.projectName='Ipact' -Dsonar.host.url=http://10.0.0.10:9000 -Dsonar.token=sqp_4a8fea5de72aa2c6b0b5b7eab781575c071f6502"

            }
        }


stage('Build Docker Image') {
                      steps {
                          script {
                            sh 'docker build -t mohamedazizbenhaha/spring-app:latest .'
                          }
                      }
                  }

                  stage('login dockerhub') {
                                        steps {
				sh 'docker login -u mohamedazizbenhaha --password dckr_pat_Rh9cY2IRelJRyTGsI5MKBY9IgVw'
                                            }
		  }
	    
	                      stage('Push Docker Image') {
                                        steps {
                                   sh 'docker push mohamedazizbenhaha/spring-app:latest'
                                            }
		  }


		   stage('Run Spring && MySQL Containers') {
                                steps {
                                    script {
                                      sh 'docker-compose up -d'
                                    }
                                }
                            }

	    



     
}

	    
        post {


       always {
		//emailext attachLog: true, body: '', subject: 'Build finished',from: 'mohamedaziz.benhaha@esprit.tn' , to: 'mohamedaziz.benhaha@esprit.tn'
            cleanWs()
       }
    }

    
	
}
       
