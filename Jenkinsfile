pipeline { //pipeline init
    agent any //jenkins agent(here set to any)
    stages { //splitting the pipeline into stages
        stage('Checkout Code') { //stage 1
            steps { //in this stage, the step is to commit code changes to the specified branch of the git url (here we use the Git API plugin)
                git(url: "https://github.com/Tamoziit/Jenkins", branch: 'main')
            }
        }

        stage('Logs') { //Stage 2
            steps { //In this stage, the step is to list out the files of the current directory in the shell
                sh 'ls -la'
            }
        }

        stage('Frontend Unit Test') { //stage 3
            steps { //Frontend unit test step > cd into frontend dir > install dependencies > run test script
                sh 'cd frontend && npm i && npm run test:unit'
            }
        }

        //Parallel stages
        stage('Parallel') { //Stage 4 - a paralel stage unlike the prev. 3 stages
            parallel { //This parallel stage runs the 2 stages (S-4.1.0 & S-4.2.0) under it simultaneously
                stage('log') {
                    steps { //S-4.1.0
                        sh 'ls -la'
                    }
                }

                stage('Backend Unit test 2') { //S-2.4.0
                    steps {
                        sh 'cd backend && npm i && npm run test:unit'
                    }
                }
            }
        }

        //Docker related stages
        stage('Build') {
            steps { //building Docker custom image
                sh 'docker build -f Dockerfile .'
            }
        }

        stage('DockerHub Login') {
            steps { //logging into DockerHub by passing the credentials as env variables.
                sh 'docker login -u $DOCKERHUB_USER -p $DOCKERHUB_PASSWORD'
            }
        }

        stage('Push') {
            steps { //pushing the custom built image into a Dockerhub repo named 'jenkins' with latest tag.
                sh 'docker push insanelytamojit/jenkins:latest'
            }
        }
    }
}