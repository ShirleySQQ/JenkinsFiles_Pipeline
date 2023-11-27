pipeline {
    agent any
environment {
        FEATURE_BRANCH = "feature/pipeline_test"
        GIT_CREDENTIALS  = credentials('9cec507e-6e56-4e51-8825-2d4f0b555388')
    }
    stages {
    stage('Delete Feature branch (Remote) if exist') {
            steps {
                sh("git branch -D ${env.FEATURE_BRANCH} 2>/dev/null || true")
            }
        }
        stage('Test Git Commands') {
            steps {
                git url: 'https://github.com/ShirleySQQ/JenkinsFiles_Pipeline.git', branch: 'main' // Replace with your actual repository URL and branch
                script {
                    sh "git config user.email 'shirley_shi@epam.com'"
                    sh "git config user.name 'ShirleySQQ'"
                    sh "git checkout -b ${env.FEATURE_BRANCH}
                    sh "git add ."
                    sh "git commit -m 'New Feature Commit'"
                }
                withCredentials([usernamePassword(credentialsId: 'github-credentials', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
                    sh """
                        set -x
                        git config credential.helper 'store --file=.git-credentials'
                        echo "https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com" > .git-credentials
                        git push -u origin ${env.FEATURE_BRANCH}
                        rm .git-credentials
                    """
                }
            }
        }
    }
}