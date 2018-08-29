pipeline {
    agent any
    stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving... YAY!'
                    archiveArtifacts artifacts: '**/target/*.war'
                    echo "I've archived all of this shit"
                }
            }
        }
    }
}
