pipeline {
    agent any
    parameters {
        string(name: 'STAGES_TO_RUN', defaultValue: 'Dependency Scan, Archive Report', description: 'Comma-separated list of stages to run')
        string(name: 'DEPENDENCY_TOOL_NAME', defaultValue: 'dependency-check', description: 'Name of the Dependency Check tool installation')
    }
    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out the code...'
                git url: 'https://github.com/RajneeshOps/attendance-api.git', branch: 'main'
            }
        }
        stage('Dependency Scan') {
            when {
                expression {
                    def stagesToRun = params.STAGES_TO_RUN.split(',').collect { it.trim() }
                    echo "Stages to run: ${stagesToRun}"
                    return stagesToRun.contains('Dependency Scan')
                }
            }
            steps {
                // Run OWASP Dependency-Check
                dependencyCheck additionalArguments: '--scan . --format ALL', odcInstallation: "${params.DEPENDENCY_TOOL_NAME}"
            }
        }
        stage('Archive Report') {
            when {
                expression {
                    def stagesToRun = params.STAGES_TO_RUN.split(',').collect { it.trim() }
                    echo "Stages to run: ${stagesToRun}"
                    return stagesToRun.contains('Archive Report')
                }
            }
            steps {

                echo "Publishing Reports"
                // Publish OWASP Dependency-Check report in HTML format
                publishHTML([
                    allowMissing: false,
                    alwaysLinkToLastBuild: true,
                    keepAll: true,
                    reportDir: '.',
                    reportFiles: 'dependency-check-report.html',
                    reportName: 'Dependency Check Report',
                    reportTitles: ''
                ])
            }
        }
    }
    post {
        always {
            echo 'Cleaning up...'
            cleanWs()
        }
    }
}
