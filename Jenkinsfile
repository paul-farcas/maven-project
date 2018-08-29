pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '13.58.49.147', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '13.58.99.221', description: 'Production Server')
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
                        sh "scp -i 1.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        timeout(time:5, unit:'DAYS'){
                            input message:'Approve PRODUCTION Deployment?'
                        }
                        sh "scp -i 1.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat/webapps"
                    }
                }
            }
        }
    }
}