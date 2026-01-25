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
		sh 'docker build -t localregistry/java-app-image:"$BUILD_NUMBER" .'
            }
        }


        stage('Trivy scan Image') {
            steps {
                echo "Scanning image using trivy"
		sh 'trivy image localregistry/java-app-image:"$BUILD_NUMBER"'
            }
        }


        stage('Image tagging') {
            steps {
                echo "tagging image copy...."
		sh 'docker tag localregistry/java-app-image:"$BUILD_NUMBER" tupeshg/maven-demo:"$BUILD_NUMBER"'
            }
        }

	stage('Image Push') {
	      steps {
        	script {
          	   docker.withRegistry('', '224e3e29-24e9-484e-8068-ac89c6cbe7d6') {
  		   docker.image("tupeshg/maven-demo:$BUILD_NUMBER").push()
  		   docker.image("tupeshg/maven-demo:$BUILD_NUMBER").push('latest')

	 }
        }
      }
    }
	
        stage('Running Container on dev') {
            steps {

		sh '''
			docker stop hamro-java-app || true
			docker rm hamro-java-app || true
		'''
                echo "Running the container........."
		sh 'docker run -d --name hamro-java-app -p 8090:8080 localregistry/java-app-image:"$BUILD_NUMBER" '
            }
        }
        stage('Deploy to dev') {
            steps {
                echo "The build $BUILD_NUMBER is about to be deployed to the dev env"
            }
        }

 	stage('deploy to prod env') {
            steps {
                 timeout(time:1, unit:'MINUTES'){
                 input message: 'Approve Production deployment?'
                 }
                echo 'prod deployment'
                sh '''
                docker stop myapp-prod || true
                docker rm  myapp-prod  || true
                docker run -d --name myapp-prod -p 8091:8080  tupeshg/maven-demo:"$BUILD_NUMBER"
                  '''
            }
        
        
    }

	post { 
        always { 
		echo "The build process has ended"
	}
       	success {
		echo "Build #$BUILD_NUMBER is successful, please go through the url"

		echo "$BUILD_URL"

		}

	failure {
            
	echo  "Build #$BUILD_NUMBER is unsuccessful, please go through the url"

	echo "$BUILD_URL"

	}
    }


}}
