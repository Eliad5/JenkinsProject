pipeline {
    agent any

    stages {
         stage('Build') {
            steps {
                sh '''
                    cd ${WORKSPACE}/scriptsdir/
                    chmod 755 *
                    date > results
                  '''
            }
        }
        stage('Bash') {
            when { anyOf {
                environment name: 'LANGUAGE', value: 'Bash'
                environment name: 'LANGUAGE', value: 'All'
            }
          }
            steps {
                 sh '''
                    cd ${WORKSPACE}/scriptsdir/
                    ./bash.sh >> results
                  '''
            }
        }
        stage('Python') {
            when { anyOf {
                environment name: 'LANGUAGE', value: 'Python'
                environment name: 'LANGUAGE', value: 'All'
            }
          }
            steps {
                sh '''
                    cd ${WORKSPACE}/scriptsdir/
                    ./python.py >> results
                  '''
            }
        }
    }
    post {
            always {
                sh '''
                   report_file="${HOME}/Documents/Deployment/report"
                   mkdir -p ${HOME}/Documents/Deployment/
                   if [ -f "${report_file}" ]
                   then
                       echo "file ${report_file} exists"
                   else
                       touch ${report_file}
                   fi
                   date >> ${report_file}
                   echo "USER-${USER} JOB_NAME-${JOB_NAME}" >> ${report_file}
                   echo "BuildNumber ${BUILD_NUMBER}" >> ${report_file}
                   cat "${WORKSPACE}/scriptsdir/results" >> ${report_file}
                   echo "##############################" >> ${report_file}
               '''
            }
        }
   }
