pipeline {
    agent any
    parameters {
        choice(
            name: 'GH_RUNNER_TAG',
            choices: ['cpc-ate-dev', 'cpc-ate-prd'],
            description: 'Runner to run tests.'
        )
        string(
            name: 'REGION',
            description: 'Region to run tests (comma-separated for multiple regions, e.g., "asse,asea")',
            defaultValue: 'asse,asea'
        )
        choice(
            name: 'SITE_TEST',
            choices: ['prd', 'sit'],
            description: 'Site to run tests.'
        )
        choice(
            name: 'BRANCH_REF',
            choices: ['main', 'sit'],
            description: 'Branch to run tests.'
        )
    }
    stages {
        stage('Parallel Execution from List') {
            parallel {
                script {
                    def stagesList = params.REGION.split(',').collect { it.trim() }
                    def parallelStages = [:]
                    
                    stagesList.each { region ->
                        parallelStages["Stage for ${region}"] = {
                            node {
                                stage("Stage for ${region}") {
                                    echo "Running tests for region: ${region}"
                                    // Add your testing commands here
                                }
                            }
                        }
                    }
                    
                    parallel parallelStages
                }
            }
        }
    }
}
