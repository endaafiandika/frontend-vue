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

        stage('Build image') {
            when {
                expression {
                    BRANCH_NAME == "deployment"
                }
            }
            steps{
               script {
                    sshPublisher(
                        publishers: [
                            sshPublisherDesc(
                                configName: 'ansible',
                                verbose: false,
                                transfers: [
                                    sshTransfer(
                                        execCommand: 'cd ansible; ansible-playbook -i hosts setup.yml',
                                    )
                                ]
                            )
                        ]
                    )
                }
            }
        }

        stage('Build Image') {
            when {
                expression {
                    BRANCH_NAME == "production"
                }
            }
            steps{
               script {
                    sshPublisher(
                        publishers: [
                            sshPublisherDesc(
                                configName: 'ansible',
                                verbose: false,
                                transfers: [
                                    sshTransfer(
                                        execCommand: 'cd ansible; ansible-playbook -i hosts frontend.yml',
                                    )
                                ]
                            )
                        ]
                    )
                }
            }
        }

         stage('deploy to development') {
            when {
                expression {
                    BRANCH_NAME == "production" || BRANCH_NAME == "deployment"
                }
            }
            steps{
               script {
                    sshPublisher(
                        publishers: [
                            sshPublisherDesc(
                                configName: 'ansible',
                                verbose: false,
                                transfers: [
                                    sshTransfer(
                                        execCommand: 'cd ansible; ansible-playbook -i hosts setup.yml',
                                    )
                                ]
                            )
                        ]
                    )
                }
            }
        }

        stage('deploy to production') {
            when {
                expression {
                    BRANCH_NAME == "production"
                }
            }
            steps{
               script {
                    sshPublisher(
                        publishers: [
                            sshPublisherDesc(
                                configName: 'ansible',
                                verbose: false,
                                transfers: [
                                    sshTransfer(
                                        execCommand: 'cd ansible; ansible-playbook -i hosts setup.yml',
                                    )
                                ]
                            )
                        ]
                    )
                }
            }
        }

        stage('Run Testing production') {
            when {
                expression {
                    BRANCH_NAME == "production"  
                }
            }
            steps{
               script {
                    sshPublisher(
                        publishers: [
                            sshPublisherDesc(
                                configName: 'ansible',
                                verbose: false,
                                transfers: [
                                    sshTransfer(
                                        execCommand: 'ansible prodserver -a "curl localhost:8080"',
                                        execTimeout: 60000,
                                    )
                                ]
                            )
                        ]
                    )
                }
            }
        } 

        stage('Run Testing Development') {
            when {
                expression {
                    BRANCH_NAME == "production" || BRANCH_NAME == "deployment"
                }
            }
            steps{
               script {
                    sshPublisher(
                        publishers: [
                            sshPublisherDesc(
                                configName: 'ansible',
                                verbose: false,
                                transfers: [
                                    sshTransfer(
                                        execCommand: 'ansible devserver -a "curl localhost:8080"',
                                        execTimeout: 60000,
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
