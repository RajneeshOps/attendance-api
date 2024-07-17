node {
    try {
        stage('Checkout') {
            // Checkout code from Git
            git branch: 'main', url: 'https://github.com/RajneeshOps/attendance-api.git'
        }

        stage('OWASP Dependency-Check Vulnerabilities') {
            // Run OWASP Dependency-Check
            dependencyCheck additionalArguments: '--scan target/ --format ALL', odcInstallation: 'dependency-check'
            
            // Publish Dependency Check report
            dependencyCheckPublisher pattern: 'dependency-check-report.html', failBuildOnCVSS: '10'
        }

        stage('Publish Dependency Check Report') {
            // Publish Dependency Check HTML report
            publishHTML([
                allowMissing: false,
                alwaysLinkToLastBuild: false,
                keepAll: true,
                reportDir: 'dependency-check-report', // Ensure this directory matches where the HTML report is generated
                reportFiles: 'dependency-check-report.html', // Ensure this file name matches the HTML report generated
                reportName: 'Dependency Check Report'
            ])
        }
    } catch (Exception e) {
        currentBuild.result = 'FAILURE'
        throw e
    }
}
