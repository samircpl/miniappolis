pipeline {
    agent any
    triggers { pollSCM '*/1 * * * *' }
    environment {
        FOO = 'bar'
    }

     stages {
        stage('Dev - Build') {
            when {
                // There are better ways but https://issues.jenkins-ci.org/browse/JENKINS-43104
                expression {
                    return env.GIT_BRANCH == "origin/dev"
                }
            }
            steps {
                script {
                        docker.withRegistry('https://registry.example.com') {
                            // Potentially use Commit Hash as identifier here
                            def customImage = docker.build("miniappolis:${env.BUILD_ID}")
            
                            //customImage.push()
                        }           
                        
                    }
                }
        }
 
        stage('Dev - Unit Tests') {
            when {
                branch '*/dev'
            }
            steps {
                sh 'echo "Testing Dev Branch"'
            }
        }
        stage('Dev - Security Scans') {
            when {
                branch '*/dev'
            }
            steps {
                sh 'echo "Testing Dev Branch"'
            }
        }

        stage('Deploy to CI') {
            when {
                branch '*/dev'
            }
            steps {
                sh 'echo "Testing Dev Branch"'
            }
        }

        stage('Master - Tag') {
            when {
                branch '*/master'
            }
            steps {
                sh 'echo "Testing MAster Branch"'
            }
        }

        stage('Master - Build/Publish') {
            when {
                branch '*/master'
            }
            steps {
                sh 'echo "Testing MAster Branch"'
            }
        }
        stage('Master - Test') {
            when {
                branch '*/master'
            }
            steps {
                sh 'echo "Testing MAster Branch"'
            }
        }

        stage('Master - Tag/Build/Publish') {
            when {
                branch '*/master'
            }
            steps {
                sh 'echo "Testing MAster Branch"'
            }
        }

        stage('Update RelMatrix with this TAG?') {

            agent none
            options {
                timeout time: 2, unit: 'DAYS' 
            }
            steps {
                script {
                    env.DO_RELEASE =  input(message:"Promote to Release Matrix?", ok:"yes")
                }
                milestone 1
            }
        }
        stage('Updating Release Candidate') {
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