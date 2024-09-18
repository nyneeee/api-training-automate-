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
        stage('Check REGION') {
            steps {
                script {
                    def regions = params.REGION.split(',').collect { it.trim() }
                    def validRegions = ['asse', 'asea']
                    def invalidRegions = regions.findAll { !validRegions.contains(it) }
                    if (invalidRegions) {
                        error "Invalid regions detected: ${invalidRegions.join(', ')}. Valid regions are: ${validRegions.join(', ')}."
                    } else {
                        echo "Regions are valid: ${regions.join(', ')}."
                    }
                    env.REGIONS = regions
                    echo "${env.REGIONS}"
                }
            }
        }
        stage('Trigger Tests REGION') {
            steps {
                script {
                    def tasks = [:]
                    for (index_region in ${env.REGIONS}) {
                        tasks["Pre-Test in ${index_region}"] = {
                            echo "Running tests for ${index_region} with parameters:"
                            echo "GH_RUNNER_TAG: ${params.GH_RUNNER_TAG}"
                            echo "REGION: ${index_region}"
                            echo "SITE_TEST: ${params.SITE_TEST}"
                            echo "BRANCH_REF: ${params.BRANCH_REF}"
                            build job: "Pre-Test Automate",
                                  parameters: [
                                      string(name: 'GH_RUNNER_TAG', value: params.GH_RUNNER_TAG),
                                      string(name: 'REGION', value: index_region),
                                      string(name: 'SITE_TEST', value: params.SITE_TEST),
                                      string(name: 'BRANCH_REF', value: params.BRANCH_REF)
                                  ]
                        }
                    }

                    parallel tasks
                }
            }
        }
    }
}