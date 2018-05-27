pipeline {
    agent any
        tools {
        maven 'LocalMaven'
    }
    parameters {
         string(name: 'tomcat_dev', defaultValue: '13.56.72.66', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '18.144.68.88', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
     }

stages{
        stage('Build'){
            
            steps {
                 env.JAVA_HOME="${tool 'jdk-8u45'}"
    env.PATH="${env.JAVA_HOME}/bin:${env.PATH}"
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
                        sh "cp -pr **/target/*.war /apps/apache-tomcat-8.5.31/webapps/"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "cp -pr **/target/*.war /apps/apache-tomcat-8.5.31-production/webapps/"
                    }
                }
            }
        }
    }
}
