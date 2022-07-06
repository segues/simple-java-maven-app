def coverageBadge = addEmbeddableBadgeConfiguration(id: "coverage", subject: "Test coverage")

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
        stage ('Badge coverage') {
            
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}
