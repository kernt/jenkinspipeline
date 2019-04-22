pipeline {
    agent any
    
    parameters { 
         string(name: 'tomcat_dev', defaultValue: 'localhost:8200', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: 'localhost:8081', description: 'Production Server')
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
                        bat "copy D:\programme\devops-environment\tomcat\apache-tomcat-9.0.12-staging\webapps"
                        # wildfly D:\programme\wildfly\16\stage
                        # sh "scp -i /home/jenkins/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                        # sh "scp -i "
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        bat "copy D:\programme\devops-environment\tomcat\apache-tomcat-9.0.12-prod\webapps"
                        # D:\programme\wildfly\16\prod
                        #sh "scp -i /home/jenkins/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                        #sh "scp -i "
                    }
                }
            }
        }
    }
}
