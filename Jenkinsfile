pipeline {
    agent any

    environment {
        FEATURE_BRANCH = "feature/pipeline_test"
        REPO_URL = "https://github.com/ShirleySQQ/JenkinsFiles_Pipeline.git"
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
                    sh """ echo '    assert True == True, "True is not True"'>> test_assert.py """
                }
            }
        }

        stage('Push to Remote') {
    steps {
    script {
                    sh "git config user.email 'shirley_shi@epam.com'"
                    sh "git config user.name 'ShirleySQQ'"
                    //sh "git checkout ${env.FEATURE_BRANCH}"
                    sh "git add ."
                    sh "git commit -m 'New Feature Commit'"
                    }
        withCredentials([usernamePassword(credentialsId: '9cec507e-6e56-4e51-8825-2d4f0b555388', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
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
stage('Checkout Feature Branch') {
            steps {
                git branch: "${env.FEATURE_BRANCH}", url: "https://github.com/ShirleySQQ/JenkinsFiles_Pipeline.git"
            }
        }
        stage('Checkout Main branch and merge feature branch ') {
            steps {
            script{
                    sh "git config user.email 'shirley_shi@epam.com'"
                    sh "git config user.name 'ShirleySQQ'"
            }
                withCredentials([usernamePassword(credentialsId: '9cec507e-6e56-4e51-8825-2d4f0b555388', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
                sh """
                        # Checkout and pull the latest main branch
                        git checkout main
                        git config credential.helper 'store --file=.git-credentials'
                        echo "https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com" > .git-credentials
                        git pull origin main
                        rm .git-credentials
                        # Merge the feature branch into the main branch
                        git merge ${env.FEATURE_BRANCH}
                        # Check for merge conflicts
                        conflicts=\$(git ls-files -u | wc -l)
                        if [ \$conflicts -gt 0 ]; then
                            echo 'Merge conflicts detected. Aborting the merge.'
                            exit 1
                        fi
                        # Push changes back to remote repository
                        git config credential.helper 'store --file=.git-credentials'
                        echo "https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com" > .git-credentials
                        git push origin main
                        rm .git-credentials
                    """
                }
            }
           }
           stage('Checkout and update feature branch') {
            steps {
                git branch: env.FEATURE_BRANCH, url: env.REPO_URL

                sh """
                    git checkout ${env.FEATURE_BRANCH}
                    git pull origin ${env.FEATURE_BRANCH}
                """
            }
        }
 stage('Delete Feature branch (Remote)') {
            steps {
            script{
                    sh "git config user.email 'shirley_shi@epam.com'"
                    sh "git config user.name 'ShirleySQQ'"
            }
                withCredentials([usernamePassword(credentialsId: '9cec507e-6e56-4e51-8825-2d4f0b555388', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')])
                 {
                     sh """
                set -x
                git config credential.helper 'store --file=.git-credentials'
                echo "https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com" > .git-credentials

                git push origin --delete ${env.FEATURE_BRANCH}

                rm .git-credentials
            """
            }
            }
        }
        stage('Setup Python Environment') {
            steps {

                sh 'pip3 install -r requirements.txt'
            }
        }

        stage('Run Tests') {
            steps {
                // Run your test commands (for example, using pytest)
                script{
                    sh "git config user.email 'shirley_shi@epam.com'"
                    sh "git config user.name 'ShirleySQQ'"
            }
                withCredentials([usernamePassword(credentialsId: '9cec507e-6e56-4e51-8825-2d4f0b555388', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')])
                 {

                     sh """
                set -x
                git config credential.helper 'store --file=.git-credentials'
                echo "https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com" > .git-credentials
                python3 -m pytest -v
                rm .git-credentials
            """
            }

            }
        }

    }

    post {
        always {
            sh("git branch -D ${env.FEATURE_BRANCH} 2>/dev/null || true")
        }
    }
}