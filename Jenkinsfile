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
            // post {
            //     success {
            //         script {
            //             // if we are in a PR
            //             if (env.CHANGE_ID) {
            //                 publishCoverageGithub(filepath:'target/site/jacoco/jacoco.xml', coverageXmlType: 'jacoco', comparisonOption: [ value: 'optionFixedCoverage', fixedCoverage: '0.65' ], coverageRateType: 'Line')
            //             }
            //         }
            //     }
            // }
        }
        stage('Record Coverage') {
            when { branch 'master' }
            steps {
                script {
                    currentBuild.result = 'SUCCESS'
                 }
                step([$class: 'MasterCoverageAction', scmVars: [GIT_URL: env.GIT_URL]])
            }
        }
        stage('PR Coverage to Github') {
            when { allOf {not { branch 'master' }; expression { return env.CHANGE_ID != null }} }
            steps {
                script {
                    currentBuild.result = 'SUCCESS'
                 }
                step([$class: 'CompareCoverageAction', publishResultAs: 'statusCheck', scmVars: [GIT_URL: env.GIT_URL]])
            }
        }
    }
    // post {
    //     always {
    //         cleanWs()
    //     }
    // }
}
