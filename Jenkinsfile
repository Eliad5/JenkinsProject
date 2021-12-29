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
                   reports_file="${HOME}/Documents/Deployment/reports"
                   mkdir -p ${HOME}/Documents/Deployment/
                   if [ -f "${reports_file}" ]
                   then
                       echo "file ${reports_file} exists"
                   else
                       touch ${reports_file}
                   fi
                   date >> ${reports_file}
                   echo "USER-${USER} JOB_NAME-${JOB_NAME}" >> ${reports_file}
                   echo "BuildNumber ${BUILD_NUMBER}" >> ${reports_file}
                   cat "${WORKSPACE}/scriptsdir/results" >> ${reports_file}
                   echo "##############################" >> ${reports_file}
               '''
            }
        }
   }
