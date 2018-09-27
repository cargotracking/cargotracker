pipeline {
    agent any
    tools { 
        maven 'maven_3-5-4' 
        jdk 'jdk8' 
    }
    stages {
        stage ('Check branch') 
            script {
                def branches = [' develop':'testing-dev', 'testing':'develop', 'testing-qa':'testing'];
                def originBranch = m.get(env.CHANGE_TARGET);
                if(originBranch != null) {
                }
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
