pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '100.77.20.141', description: 'Staging Server')
    }

    triggers {
         pollSCM('* * * * *') // Polling Source Control
     }

stages{
        stage('Build'){
            steps {
               # sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        sh "scp  **/target/*.war test_user@${params.tomcat_dev}:/var/lib/tomcat/webapps"
                    }
                }
        stage ("Deploy to Production"){
                    steps {
                        sh "scp  **/target/*.war test_user@${params.tomcat_dev}:/var/lib/tomcat/webapps"
                    }
                }
            }
        }
    }
}
