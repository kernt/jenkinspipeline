pipeline {
    agent any
    
    parameters { 
         string(name: 'tomcat_dev', defaultValue: 'localhost:9090', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: 'localhost:8100', description: 'Production Server')
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
                        bat "copy E:\programme\apache-tomcat-9.0.12-windows-x64\apache-tomcat-9.0.12-stage\webapps E:\programme\apache-tomcat-9.0.12-windows-x64\apache-tomcat-9.0.12-prod\webapps"
                        # sh "scp -i /home/jenkins/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        bat "copy E:\programme\apache-tomcat-9.0.12-windows-x64\apache-tomcat-9.0.12-stage\webapps E:\programme\apache-tomcat-9.0.12-windows-x64\apache-tomcat-9.0.12-prod\webapps"
                        #sh "scp -i /home/jenkins/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}
