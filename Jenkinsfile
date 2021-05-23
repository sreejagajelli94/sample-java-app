pipeline {
    agent any
    environment {
        APP_HOME='/home/app'
        PRAGRA_BATCH='devs'
    }
    options { 
        quietPeriod(30) 
    }
    parameters { 
            choice(name: 'ENV_TO_DEPLOY', 
            choices: ['ST', 'UAT', 'STAGING'], description: 'Select a Env to deploy') 

             booleanParam(name: 'RUN', defaultValue: true, description: 'SELECT TO RUN')
    }
    triggers {
        pollSCM('* * * * *')
    }
    tools{
        maven  'm3'
        jdk 'jdk11'
    }

    stages {
        stage('Git CheckoutOut'){
            environment{
                GIT_REPO='https://github.com/sreejagajelli94/sample-java-app.git'
            }
            steps {
                git "${GIT_REPO}"
            }
        }
        stage('Compile') {
            steps {
                sh 'mvn compile'
            }
        }

        stage('Unit Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Package') {
            steps {
                sh 'mvn package'
            }
        }
        stage('Artifactory') {
            steps {
                sh 'mvn install'
            }
        }
    }

    post {
        always {
            echo 'ALL GOOD '
            echo "${BRANCH_NAME}  : ${JOB_NAME}"
        }
    }


}


// && 