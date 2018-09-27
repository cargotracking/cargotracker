pipeline {
    agent any
    tools { 
        maven 'maven_3-5-4' 
        jdk 'jdk8' 
    }
    stages {
        stage ('Check branch') {
            def branches = [ develop:'testing-dev', testing:'develop', testing-qa:'testing'];
            def originBranch = m.get(env.CHANGE_TARGET);
            if(originBranch != null) {
                sh '''
                   last_hash=$(git log -n 1 --pretty=format:'%h')

                   # Ensure the commit comes is present on the desired previous branch
                   git clone -b $originBranch --single-branch https://github.com/cargotracking/cargotracker.git
                   cd cargotracker
                   git checkout $last_hash
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
