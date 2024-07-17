node {
    try {
        stage('Checkout') {
            // Checkout code from Git
            git branch: 'main', url: 'https://github.com/RajneeshOps/attendance-api.git'
        }

        stage('OWASP Dependency-Check Vulnerabilities') {
            dependencyCheck additionalArguments: ''' 
                    -o './'
                    -s './'
                    -f 'ALL' 
                    --prettyPrint''', odcInstallation: 'OWASP Dependency-Check Vulnerabilities'
            dependencyCheckPublisher pattern: 'dependency-check-report.xml'
        }

        stage('Publish Dependency Check Report') {
            // Publish Dependency Check report
            publishHTML([
                allowMissing: false,
                alwaysLinkToLastBuild: false,
                keepAll: true,
                reportDir: 'dependency-check-report',
                reportFiles: 'dependency-check.html',
                reportName: 'Dependency Check Report'
            ])
        }
    } catch (Exception e) {
        currentBuild.result = 'FAILURE'
        throw e
    }
}
