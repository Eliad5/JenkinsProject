pipeline {
    agent any


    stages {
        stage('Build') {
            steps {
                echo "Build process.."
                sh '''
                    cd ${WORKSPACE}/scripts/
                    chmod 755 *
                    date > results
                  '''
            }
        }
        
        stage('BASH') {
            steps {
              when { anyOf {
                enviromen name: "LANGUAGE", value "All"
                enviromen name: "LANGUAGE", value "BASH"
              }
              }
                echo "Hello BASH"
            }
        }
    }
}

    stages {
        stage('Python') {
            steps {
              when { anyOf {
                enviromen name: "LANGUAGE", value "All"
                enviromen name: "LANGUAGE", value "Python"
                print("Hello Python")
            }
        }
    }
}

   post {
            always {
                echo 'Saving Results process..'
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
                   cat "${WORKSPACE}/scripts/results" >> ${report_file}
                   echo "##############################" >> ${report_file}
               '''
            }
        }
   }
