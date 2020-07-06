pipeline {
    agent any
    triggers { pollSCM 'H/2 * * * *' }
    environment {
        FOO = 'bar'
    }

    stages {
        stage('Build') {
            when {
                branch 'master' 
            }
            steps {
                git url: 'git@github.com:samircpl/miniappolis.git'
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
