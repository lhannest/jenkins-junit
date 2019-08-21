pipeline {

    agent any

    triggers {
        cron('H */8 * * *') //regular builds
        pollSCM('* * * * *') //polling for changes, here once a minute
    }

    stages {
//        stage('Checkout') {
//            steps { //Checking out the repo
//                checkout changelog: true, poll: true, scm: [$class: 'GitSCM', branches: [[name: '*/master']], browser: [$class: 'BitbucketWeb', repoUrl: 'https://web.com/blah'], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'git', url: 'ssh://git@git.giturl.com/test/test.git']]]
//            }
//        }
        stage('Unit & Integration Tests') {
            steps {
                script {
                    try {
                        sh './gradlew clean test --no-daemon' //run a gradle task
                    } finally {
                        junit '**/build/test-results/test/*.xml' //make the junit test results available in any case (success & failure)
                    }
                }
            }
        }
    }
    post {
        always {
        	junit '**/target/*.xml'
        	archive (includes: '**/target/*.xml')

			  // publish html
			  // snippet generator doesn't include "target:"
			  // https://issues.jenkins-ci.org/browse/JENKINS-29711.
			  publishHTML (target: [
			      allowMissing: false,
			      alwaysLinkToLastBuild: false,
			      keepAll: true,
			      reportDir: 'coverage',
			      reportFiles: 'index.html',
			      reportName: "RCov Report"
			    ])
        }
    }
}
