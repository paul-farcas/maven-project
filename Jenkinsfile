pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '52.14.212.83', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '18.216.136.110', description: 'Production Server')
         string(name: 'myKey', defaultValue: "C:\\super.pem", description: 'Staging Server SSH Key')
    }

    triggers {
         pollSCM('* * * * *')
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
            stages{
                stage ('Deploy to Staging'){
                    steps {
                        sh "scp -i ${params.myKey} **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        timeout(time:5, unit:'MINUTES'){
                            input message:'Approve PRODUCTION Deployment?'
                        }
                        sh "scp -i ${params.myKey} **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat/webapps"
                    }
                }
            }
        }
    }
}