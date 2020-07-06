pipeline {
    agent any
    triggers { pollSCM '*/1 * * * *' }
    environment {
        FOO = 'bar'
    }

    stages {
        stage('Feature Branch Build') {
            when {
                not {
                    branch '*/master'
                }
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
        stage('Feature Branch Test') {
            when {
                not {
                    branch '*/master'
                }
            }
            steps {
                sh 'ls -lisat'
            }
        }
        stage('Opt. Deploy to CI') {
            when {
                not {
                    branch '*/master'
                }
            }
            steps {
                sh 'ls -lisat'
            }
        }
    }
}



// ------------
// Pre-merge 

//   * Build
//   * Test
//   * PR - Deploy to CI


// Post-merge

//  * build
//  * test
//  * (manual) Tag
//  * (manual) Deploy Dogfood


// Beyond (manual)

//  * Deploy to Beta
//  * Deploy to Gamma
//  * Deploy to Kappa