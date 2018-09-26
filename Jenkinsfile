pipeline {
    agent any
    tools { 
        maven 'maven_3-5-4' 
        jdk 'jdk8' 
    }
    stages {
        stage ('Check branch') {
            when {
                environment name: 'CHANGE_TARGET', value: 'testing-dev'
            }
            steps {
                echo 'Develop branch'
                sh '''
                   last_hash=$(git log -n 1 --pretty=format:'%h')

                   git clone -b testing-dev --single-branch https://github.com/cargotracking/cargotracker.git
                   cd cargotracker
                   git checkout $last_hash
                   result=$?
                   echo "Result: $result"
                '''
            }
        }
        stage ('Initialize') {
            steps {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                ''' 
            }
        }
        stage ('Build') {
            steps {
                sh 'mvn install -DskipTests' 
            }
        }
    }
}
