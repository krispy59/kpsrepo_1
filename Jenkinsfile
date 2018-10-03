pipeline {
    agent none
    stages {
        stage('SCM') {
            //agent{label "MAC"}
            agent{label "linux_jnlp"}
            steps {
                sh "echo hello from this stage"
                sh "echo hello from this stage >output.txt"
                sh 'uname -a'
                sh 'uname -a >>output.txt'
                sha1("output.txt")
                touch("nextfile.txt")
            }
        }
        stage('CP1') {
            steps {
                checkpoint('STAGE1')
            }
        }
        stage('BUILD') {
            agent{label 'linux_jnlp'}
            steps {
                script {
                    if ( fileExists('trunk/fileA.txt') ) {
                        stage('FILE EXISTS') {
                          sh 'env'
                          sh 'env >>output.txt'
                        }
                    } else {
                        stage('NOT EXIST') {
                            sh 'cat /etc/passwd'
                            sh 'cat /etc/passwd >>output.txt'
                        }
                    }
                }
                stash includes: 'output.txt', name: 'output_files'
            }
        }
        stage('CP2') {
            steps {
                checkpoint('STAGE2')
            }
        }
        stage('RESULTS') {
            agent{label 'linux_jnlp'}
            steps {
                unstash 'output_files'
                sh 'pwd'
                sh 'cat output.txt'
            }
        }
        stage('CP3') {
            steps {
                checkpoint('STAGE3')
            }
        }
    }
}
