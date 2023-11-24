pipeline {
    agent any

    environment {
        FEATURE_BRANCH = "feature/branch_one"
    }

    stages {
        stage('GIT Checkout') {
            steps {
                git changelog: false, poll: false, url: 'https://github.com/ShirleySQQ/JenkinsFiles_Pipeline.git', branch: 'main'
            }
        }

        stage('Create Feature Branch') {
            steps {
                script {
                    sh("git checkout -b ${env.FEATURE_BRANCH}")
                }
            }
        }

        stage('Make Changes') {
            steps {
                script {
                    // Replace this step with your actual changes in your Jenkinsfile
                    sh("echo 'Made changes' >> test_assert.py")
                }
            }
        }

        stage('Push and Create PR') {
            steps {
                // Configure Git user (if needed)
                sh("git config user.name 'ShirleySQQ'")
                sh("git config user.email 'shirley_shi@epam.com'")

                script {
                    // Commit and push the changes to the feature branch
                    sh("git add test_assert.py")
                    sh("git commit -m 'Sample commit'")
                    sh("git push -u origin ${env.FEATURE_BRANCH}")

                    // Create a PR using the GitHub CLI
                    sh("gh pr create --title 'Sample PR' --body 'Testing automated PR creation' --head ${env.FEATURE_BRANCH} --base main")
                }
            }
        }

        stage('Checkout Main branch') {
            steps {
                git branch: 'main', url: 'https://github.com/ShirleySQQ/JenkinsFiles_Pipeline.git'
            }
        }

        stage('Setup Python Environment') {
            steps {
                script {
                    // Create a virtual environment and activate it
                    sh 'python3 -m venv venv'
                    sh '. venv/bin/activate'
                }
                sh 'pips install -r requirements.txt'
            }
        }

        stage('Run Tests') {
            steps {
                // Run your test commands (for example, using pytest)
                sh 'pytest -v'
            }
        }

    }

    post {
        always {
            sh 'deactivate'
            sh 'rm -r venv'
        }
    }
}