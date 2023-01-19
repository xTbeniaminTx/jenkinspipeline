pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '192.168.1.62', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '192.168.1.62', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
     }

stages{
        stage('Build'){
            steps {
                bat 'mvn clean package'
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
                        bat "scp **/target/*.war root@${params.tomcat_dev}:/home/ben/apache/apache-production/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        bat "scp **/target/*.war root@${params.tomcat_prod}:/home/ben/apache/apache-staging/webapps"
                    }
                }
            }
        }
    }
}