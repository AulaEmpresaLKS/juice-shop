pipeline {
    agent { 
        label 'zuvmljenson02'
    }
    environment {
        BUILD_IMAGE = "registry.lksnext.com/devsecops/node-22:2.0"
        SONAR_HOST_URL = "https://sonarqubeenterprise.devops.lksnext.com/"
        SONAR_TOKEN = credentials('sonarenterprise-analysis-token')
        SONAR_BRANCH = env.BRANCH_NAME
    }
    stages {
        stage('Build') {
            steps {
                script {
                    sh '''
                        docker run --rm \
                            -v ./:/app \
                            -e JOB_ACTION=compile \
                            -e NODE_CMD=install \
                            $BUILD_IMAGE \
                    '''
                }
            }
        }
        stage('Sonar') {
            steps {
                script {
                    sh '''
                        docker run --rm \
                            -v ./:/app \
                            -e SONAR_HOST_URL=$SONAR_HOST_URL \
                            -e SONAR_TOKEN=$SONAR_TOKEN \
                            -e JOB_ACTION=sonar \
                            -e SONAR_BRANCH_NAME=$SONAR_BRANCH \
                            $BUILD_IMAGE
                    '''
                }
            }
        }
    }
    post {
        always {
            deleteDir()
        }
    }
}
