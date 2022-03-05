pipeline {
  environment {
    registry = '026234048714.dkr.ecr.us-east-1.amazonaws.com/nike-hello-world'
    registryCredential = 'c633fe47-56da-46b1-9345-7207557ba827'
    dockerImage = 'nike-hello-world'
  }
    agent any
    triggers {
        pollSCM '* * * * *'
    }
    stages {
        stage('Build') {
            steps {
                echo "Building"
                bat 'gradlew build'
            }
        }
		stage ('Build docker image') {
    		steps {
    			echo "Docker step"
        		bat 'docker build --build-arg JAR_FILE=build/libs/rest-service-0.0.1-SNAPSHOT.jar -t nike-hello-world .'
        		bat 'docker tag nike-hello-world aws_account_id.dkr.ecr.region.amazonaws.com/nike-hello-world'
    			}
		}
    	stage('Deploy image') {
        steps {
        		echo "Docker step"
            	script	{
                	docker.withRegistry("https://" + registry, "ecr:us-east-1:" + registryCredential) {
                    bat 'docker push nike-hello-world:latest'
               	 }
            }
        }
    }
}
}
