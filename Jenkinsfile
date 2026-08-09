@Library('shared') _

pipeline {

    agent {
        label "vinod"
    }

    stages {

        stage("Hello") {
            steps {
                script {
                    hello()
                }
            }
        }

        stage("Code") {
            steps {
                script {
                    clone(
                        "https://github.com/faizahmd2004-al/django-notes-app.git",
                        "main"
                    )
                }
            }
        }

        stage("Build1") {
            steps {
                script {
                    dockerBuild("notes-apps")
                }
            }
        }

        stage("Push to DockerHub") {
            steps {
                script {
                    withCredentials([
                        usernamePassword(
                            credentialsId: 'DockerHubCred',
                            usernameVariable: 'DOCKER_USER',
                            passwordVariable: 'DOCKER_PASS'
                        )
                    ]) {

                        sh '''
                            echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        '''

                        dockerPush("notes-apps", DOCKER_USER)
                    }
                }
            }
        }

        stage("Deploy") {
            steps {
                script {
                    dockerDeploy()
                }
            }
        }
    }
}
