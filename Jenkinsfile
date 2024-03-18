pipeline {
    agent any
    tools {
        nodejs 'my-nodejs'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout your code from the GitHub repository
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install' // Install project dependencies
            }
        }

        stage('Run Unit Tests') {
            steps {
                sh 'npm test' // Run unit tests
            }
        }

        stage('Build') {
            steps {
                sh 'npm run build' // Build the React app
            }
        }

        stage('Publish Artifacts') {
            steps {
                archiveArtifacts artifacts: '**/build/**', allowEmptyArchive: true // Archive build artifacts
            }
        }
    }

    post {
        always {
            junit 'junit.xml' // Publish JUnit test reports
            script {
                def githubStatus = new org.jenkinsci.plugins.github_status.GitHubStatus()
                def commitSHA = env.GIT_COMMIT
                def prNumber = env.CHANGE_ID // CHANGE_ID contains PR number if triggered by a PR event
                def githubToken = credentials('GITHUB_ACCESS_TOKEN')

                // Determine build status
                def status = 'success' // or 'failure' based on your build result

                // Update GitHub status
                githubStatus.createCommitStatus(
                    context: 'Jenkins',
                    description: 'Jenkins build status',
                    repoOwner: 'mano-balakrishnan', // GitHub repository owner
                    repoName: 'feb16', // GitHub repository name
                    commitSha: commitSHA,
                    status: status,
                    accessToken: githubToken
                )
            }
        }
    }
}
