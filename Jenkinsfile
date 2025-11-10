pipeline {
    agent any
    environment {
        BLUE_DIR = "/var/www/blue"
        GREEN_DIR = "/var/www/green"
        LIVE_DIR = "/var/www/live"
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'git@github.com:Happy-Harsha/maven1.git'
            }
        }
        stage('Deploy') {
            steps {
                script {
                    // Determine which environment is inactive
                    if (fileExists("${LIVE_DIR}/current_env") && readFile("${LIVE_DIR}/current_env").trim() == "blue") {
                        env.DEPLOY_DIR = GREEN_DIR
                        writeFile file: "${LIVE_DIR}/current_env", text: "green"
                    } else {
                        env.DEPLOY_DIR = BLUE_DIR
                        writeFile file: "${LIVE_DIR}/current_env", text: "blue"
                    }

                    // Deploy application to the inactive environment
                    sh "rsync -av --delete ./ ${DEPLOY_DIR}/"

                    // Switch live symlink
                    sh "ln -sfn ${DEPLOY_DIR} ${LIVE_DIR}/app"
                }
            }
        }
    }
}
