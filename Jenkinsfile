pipeline {
    agent any
    tools { 
        maven 'maven_3-5-4' 
        jdk 'jdk8' 
    }
    stages {
        stage ('Check branch') {
            steps {
                script {
                    def branches = ['develop':'testing-dev', 'testing':'develop', 'testing-qa':'testing'];
                    env.originBranch = branches.get(env.CHANGE_TARGET);
                    if(originBranch != null) {
                        sh '''
                            last_hash=$(git log -n 1 --pretty=format:'%h')
    
                            # Ensure the commit comes is present on the desired previous branch
                            echo $originBranch
                            git clone -b ${env.originBranch} --single-branch https://github.com/cargotracking/cargotracker.git
                            cd cargotracker
                            git checkout $last_hash
                        '''
                    } else {
                        println "No need to check branches!"
                    }
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
