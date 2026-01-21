pipeline {
    agent any

    stages {
        stage('Compile Stage') {
            steps {
                echo 'Compiling the code'
		sh 'mvn clean package'
            }
        

	post { 
                success { 
        	    echo 'The compile has been successfull, now archeiving the artifact'
                    archiveArtifacts artifacts: '**/*.war', followSymlinks: false
		}
		failure{
		   echo "Khuching paryo"
		}

	}
	}
        stage('Unit Test Stage') {
            steps {
			echo "Test cases are running here in this stage"
		}
            }

        stage('Building Image') {
            steps {
                echo "Building image from the dockerfile"
		sh 'docker image -t localregistry/java-app-image:"$BUILD_NUMBER . " '
            }
        }


        stage('Running Container') {
            steps {
                echo "Running the container........."
		sh 'docker run -d --name hamro-java-app -p 8090:8080 localregistry/java-app-image:"$BUILD_NUMBER" '
            }
        }
        stage('Deploy to dev') {
            steps {
                echo "The build $BUILD_NUMBER is about to be deployed to the dev env"
            }
        }
        
        
    }
}

