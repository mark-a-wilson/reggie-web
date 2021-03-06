pipeline {
    agent none
    options {
        disableResume()
    }
    stages {
        stage('Build') {
            agent { label 'build' }
            steps {
                echo "Aborting all running jobs ..."
                script {
                    abortAllPreviousBuildInProgress(currentBuild)
                }
                echo "Building ..."
                sh "export 'DEBUG=*' && .pipeline/cli.sh build -- --pr=${CHANGE_ID}"
            }
        }
        stage('Deploy (DEV)') {
            agent { label 'deploy' }
            steps {
                echo "Deploying to dev..."
                sh ".pipeline/cli.sh deploy -- --pr=${CHANGE_ID} --env=dev"
            }
        }
        stage('Deploy (TEST)') {
            agent { label 'deploy' }
            input {
              message "Deploy to test?"
              ok "Yes."
            }
            steps {
                echo "Deploying to test ..."
                sh ".pipeline/cli.sh deploy -- --pr=${CHANGE_ID} --env=test"
            }
        }
        stage('Deploy (PROD)') {
            agent { label 'deploy' }
            input {
              message "Deploy to prod?"
              ok "Yes."
            }
            steps {
                echo "Deploying to prod ..."
                sh ".pipeline/cli.sh deploy -- --pr=${CHANGE_ID} --env=prod"
            }
        }
    }
}