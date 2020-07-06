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


        stage('Deployt to CI?') {

            agent none
            options {
                timeout time: 2, unit: 'DAYS' 
            }
            steps {
                script {
                    env.DO_RELEASE =  input(message:"Deploy to CI?", ok:"yes")
                }
                milestone 1
            }
        }
        stage('Deploy to CI') {
            agent any
            when {
            beforeAgent true
            environment name: 'DO_RELEASE', value: 'yes'
            }
            steps {
            lock('release') {
                milestone 2
                sh 'echo "Trigger CloudBuild here to deploy to EKS"'
            }
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