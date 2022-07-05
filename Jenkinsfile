pipeline {
    agent any
    options {
      disableConcurrentBuilds()
    }
    stages {
        stage ('Test') {
            steps {
                sh 'mvn verify'
            }
        }
        stage ('Jacoco') {
            steps {
                echo 'Code Coverage'
                jacoco()
            }
            post {
                success {
                    script {
                        // if we are in a PR
                        if (env.CHANGE_ID) {
                            publishCoverageGithub(filepath:'target/site/jacoco/jacoco.xml', coverageXmlType: 'jacoco', comparisonOption: [ value: 'optionFixedCoverage', fixedCoverage: '0.65' ], coverageRateType: 'Line')
                        }
                    }
                }
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}
