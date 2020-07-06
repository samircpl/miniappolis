pipeline {
    agent any
    triggers { pollSCM 'H/1 * * * *' }
    environment {
        FOO = 'bar'
    }

    stages {
        stage('Build') {
            when {
                branch '*/master' 
            }
            steps {
                script {
                        docker.withRegistry('https://registry.example.com') {
                            def customImage = docker.build("miniappolis:${env.BUILD_ID}")
            
                            //customImage.push()
                        }           
                        
                    }
                }
        }
        stage('Test') {
            steps {
                sh 'ls -lisat'
            }
        }
    }
}
