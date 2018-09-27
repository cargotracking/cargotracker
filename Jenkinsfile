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
                    def originBranch = branches.get(env.CHANGE_TARGET);
                    if(originBranch != null) {
                        withEnv(["ORIGIN_BRANCH=$originBranch"]) {
                            sh '''
                                last_hash=$(git log -n 1 --pretty=format:'%h')
    
                                # Ensure the commit comes is present on the desired previous branch
                                echo Cloning ${ORIGIN_BRANCH}
                                # Remove directory if exists
                                rm -Rf cargotracker
                                git clone -b ${ORIGIN_BRANCH} --single-branch https://github.com/cargotracking/cargotracker.git
                                cd cargotracker
                                git checkout $last_hash
                            '''
                         }
                    } else {
                        println "No need to check branches! (target is $env.CHANGE_TARGET)"
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
