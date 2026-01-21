pipeline {
    agent any

    stages {
        stage('Greeting') {
            steps {
                echo 'Hello Class'
            }
        }
        stage('Attendance') {
            steps {
                sh ''' 
                    for i in {1..10}; do
                        if [$i % 3 == 0]; then
                            echo "Student $i present "
                        fi
                    done
                '''
            }
        }
        stage('Departure') {
            steps {
                echo "Go home everyone the $BUILD_NUMBER is over"
            }
        }
        
        
    }
}

