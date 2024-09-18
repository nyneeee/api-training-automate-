pipeline {
    agent any
    environment {
        REGION = "asse"
        SITE_TEST = "main"
        PATH = "/opt/homebrew/bin:${env.PATH}"
    }
    stages {
        stage('Test Checkout GitHub') {
            steps {
                script {
                    checkout scmGit(branches: [[name: "*/${env.SITE_TEST}"]], extensions: [], userRemoteConfigs: [[url: 'https://github.com/nyneeee/api-training-automate.git']])
                }
                sh 'ls -lrt'
            }
        }
        stage('Test Grep CMD Run PreTest Robot') {
            steps {
                script {
                    def CMD_RUN_PRE_TEST = sh(
                        script: """
                            grep 'cmd_run_robot_${REGION}_${SITE_TEST}' ./Config/config_cicd_run_robot.yml | cut -d ':' -f 2-
                        """,
                        returnStdout: true
                    ).trim()  
                    echo "CMD_RUN_PRE_TEST: ${CMD_RUN_PRE_TEST}"
                    env.CMD_RUN_PRE_TEST = CMD_RUN_PRE_TEST
                }
            }
        }
        stage('Run Robot PreTest') {
            steps {
                sh(script: """
                    cd Testsuites
                    ${env.CMD_RUN_PRE_TEST}
                """)
            }
        }
    }
}
