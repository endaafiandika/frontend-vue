def builderDocker
def CommitHash

pipeline {

    agent any

    parameters {
        booleanParam(name: 'RUNTEST', defaultValue: true, description: 'Toggle this value fro testing')
    }

    stages {

        stage('Build Project') {
            steps {
                nodejs("node12") {
                    sh 'yarn install'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    CommitHash = sh (script : "git log -n 1 --pretty=format:'%H'", returnStdout: true)
                    builderDocker = docker.build("endaafiandika/frontend-vue:${CommitHash}")
                }
            }
        }

        stage('Run Testing') {
            when {
                expression {
                    params.RUNTEST
                }
            }
            steps {
                script {
                    builderDocker.inside {
                        sh 'echo passed ${BRANCH_NAME}'
                    }
                }
            }
        }

        stage('Push Image') {
            when {
                expression {
                    params.RUNTEST
                }
            }

            steps {
                script {
                    builderDocker.push("${env.GIT_BRANCH}")
                }
            }
        }

        stage('Deploy') {
            when {
                expression {
                    BRANCH_NAME == "deployment"
                }
            }
            steps {
                script {
                    sshPublisher(
                        publishers: [
                            sshPublisherDesc(
                                configName: 'deployment',
                                verbose: false,
                                transfers: [
                                    sshTransfer(
                                        execCommand: 'docker pull endaafiandika/frontend-vue:master; docker kill frontend-vue:master;docker run -d --rm -p 8080:80 --name frontend endaafiandika/frontend-vue:master',
                                        execTimeout: 120000,
                                    )
                                ]
                            )
                        ]
                    )
                }
            }
        }

        stage('Production') {
            when {
                expression {
                    BRANCH_NAME == "production"
                }
            }
            steps {
                script {
                    sshPublisher(
                        publishers: [
                            sshPublisherDesc(
                                configName: 'production',
                                verbose: false,
                                transfers: [
                                    sshTransfer(
                                        execCommand: 'docker pull endaafiandika/frontend-vue:master; docker kill frontend-vue:master;docker run -d --rm -p 8080:80 --name frontend endaafiandika/frontend-vue:master',
                                        execTimeout: 120000,
                                    )
                                ]
                            )
                        ]
                    )
                }
            }
        }        
    }
}

