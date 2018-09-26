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
                echo sh(returnStdout: true, script: "git log -n 1 --pretty=format:'%h'").trim()
                git branch: 'testing',
                    url: 'https://github.com/cargotracking/cargotracker.git'
                sh '''
                   last_hash=git log -n 1 --pretty=format:'%h'
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
