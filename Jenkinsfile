node {
    try {
        stage('Checkout') {
            // Checkout code from Git
            git branch: 'main', url: 'https://github.com/RajneeshOps/attendance-api.git'
        }

        stage('Dependency Scan') {
            // Run OWASP Dependency-Check
            dependencyCheck additionalArguments: '--format HTML', odcInstallation: 'dependency-check'
        }

        stage('Publish Dependency Check Report') {
            // Publish the Dependency Check Report
            dependencyCheckPublisher pattern: '**/dependency-check-report.html'
        }
    } catch (Exception e) {
        // Handle any exceptions that occur during the pipeline execution
        currentBuild.result = 'FAILURE'
        throw e
    }
}
