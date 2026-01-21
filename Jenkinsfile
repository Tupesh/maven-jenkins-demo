pipeline {
    agent any

    stages {
        stage('Compile Stage') {
            steps {
                echo 'Compiling the code'
		sh 'mvn clean package'
            }
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
        stage('Unit Test Stage') {
            steps {
		steps{
			echo "Test cases are running here in this stage"
		}
            }
        }
        stage('Deploy to dev') {
            steps {
                echo "The build $BUILD_NUMBER is about to be deployed to the dev env"
            }
        }
        
        
    }
}

