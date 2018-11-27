pipeline {
    agent any
    
    parameters { 
         string(name: 'tomcat_dev', defaultValue: '54.185.201.217', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '54.200.249.223', description: 'Production Server')
    } 

    triggers {
         pollSCM('* * * * *') // Polling Source Control
     }

stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
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
                        sh "scp -i /home/indulkat/tejas01.pem **/target/*.war ec2-user@${params.tomcat_dev}:/root/apache-tomcat-8.0.53/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i /home/indulkat/tejas01.pem **/target/*.war ec2-user@${params.tomcat_prod}:/root/apache-tomcat-8.0.53/webapps"
                    }
                }
            }
        }
    }
}
