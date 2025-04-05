pipeline {
    agent any  // Runs on any available Jenkins agent

    environment {
        REPO_URL = 'https://github.com/ranjith-natarajan-git/jenkinsdemo.git'
        BRANCH = 'main'
        SLACK_CHANNEL = '#jenkins-alerts'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: "${BRANCH}", url: "${REPO_URL}"
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'  // Replace with 'mvn install' for Java projects
            }
        }

        stage('Build Application') {
            steps {
                sh 'npm run build'  // Replace with build command for your stack
            }
        }

        stage('Run Automation Tests') {
            steps {
                sh 'npm test'  // Replace with 'mvn test' for Java projects
            }
        }

        stage('Publish Test Reports') {
            steps {
                junit '**/test-results.xml'  // Adjust based on test framework
            }
        }

        stage('Deploy (Optional)') {
            when {
                branch 'main'
            }
            steps {
                sh './deploy.sh'  // Deployment script (Docker, Kubernetes, etc.)
            }
        }
    }

    post {
        success {
            echo 'Build Successful!'
            slackSend channel: "${SLACK_CHANNEL}", message: "✅ Build & Tests Passed!"
        }
        failure {
            echo 'Build Failed!'
            slackSend channel: "${SLACK_CHANNEL}", message: "❌ Build Failed!"
        }
    }
}
