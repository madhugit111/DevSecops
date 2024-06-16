pipeline {
  agent any
  tools { 
        maven 'Maven_3_8_4'  
    }
   stages{
    stage('CompileandRunSonarAnalysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=asgbuggywebapp3221_asgbuggywebapp -Dsonar.organization=asgbuggywebapp3221 -Dsonar.host.url=https://sonarcloud.io -Dsonar.token=a6623808c9dc1041b4027a1e8ae6e81246d27653'
			}
    }
	stage('RunSCAAnalysisUsingSnyk') {
            steps {		
				withCredentials([string(credentialsId: 'Snyk_token', variable: 'SNYK_TOKEN')]) {
					sh 'mvn snyk:test -fn'
				}
			}
    }
	stage('Build') { 
            steps { 
               withDockerRegistry([credentialsId: "Docker_Login", url: ""]) {
                 script{
                 app =  docker.build("asg")
                 }
               }
            }
    }

	stage('Push') {
            steps {
                script{
                    docker.withRegistry('https://471112694879.dkr.ecr.us-east-1.amazonaws.com/asg', 'ecr:us-east-1:aws-credentials') {
                    app.push("latest")                                                                        
                    }
                }
            }
    	}
	
   }
}
