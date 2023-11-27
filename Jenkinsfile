pipeline {
    agent any

    environment {
        FEATURE_BRANCH = "feature/pipeline_test"
        GIT_CREDENTIALS_ID = '9cec507e-6e56-4e51-8825-2d4f0b555388'
    }

    stages {
    stage('Delete Feature branch (Remote) if exist') {
            steps {
                sh("git branch -D ${env.FEATURE_BRANCH} 2>/dev/null || true")
            }
        }

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
                    //sh("git push -u origin ${env.FEATURE_BRANCH}")
                    sh( """git config credential.helper 'store --file=.git-credentials'
                    echo "https://\${GIT_USERNAME}:\${GIT_PASSWORD}@github.com" > .git-credentials
                    git push -u origin ${env.FEATURE_BRANCH}
                    rm .git-credentials """)

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
 stage('Delete Feature branch (Remote)') {
            steps {
                sh "git push origin --delete ${env.FEATURE_BRANCH}"
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
            sh "git branch -d ${env.FEATURE_BRANCH}"
        }
    }
}