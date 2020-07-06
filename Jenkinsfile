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
            // Don't allocate an agent because we don't want to block our
            // slaves while waiting for user input.
            agent none
            // Play with this for branch names
            // when {
            // // You forgot the 'env.' in your example above ;)
            // expression { env.BRANCH_NAME ==~ /^qa[\w-_]*$/ }
            // }
            options {
            // Optionally, let's add a timeout that we don't allow ancient
            // builds to be released.
            timeout time: 14, unit: 'DAYS' 
            }
            steps {
            // The input statement has to go to a script block because we
            // want to assign the result to an environment variable. As we 
            // want to stay as declarative as possible, we put noting but
            // this into the script block.
            script {
                // Assign the 'DO_RELEASE' environment variable that is going
                //  to be used in the next stage.
                env.DO_RELEASE =  input {   message "Deploy to CI?"
                                            ok "yes"
                                        }
            }
            // In case you approved multiple pipeline runs in parallel, this
            // milestone would kill the older runs and prevent deploying
            // older releases over newer ones.
            milestone 1
            }
        }
        stage('Deployt to CI') {
            // We need a real agent, because we want to do some real work.
            agent any
            when {
            // Evaluate the 'when' directive before allocating the agent.
            beforeAgent true
            // Only execute the step when the release has been approved.
            environment name: 'DO_RELEASE', value: 'yes'
            }
            steps {
            // Make sure that only one release can happen at a time.
            lock('release') {
                // As using the first milestone only would introduce a race 
                // condition (assume that the older build would enter the 
                // milestone first, but the lock second) and Jenkins does
                // not support inter-stage locks yet, we need a second 
                // milestone to make sure that older builds don't overwrite
                // newer ones.
                milestone 2

                // Now do the actual work here.
                sh 'echo "deploying to CI"'
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