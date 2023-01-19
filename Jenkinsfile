pipeline {
    agent any

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

        stage ('Deploy to Staging'){
              steps {
                        bat "winscp scp:**/target/*.war root@192.168.1.62:/home/ben/apache/apache-staging/webapps"
                    }
              }

    }
}