pipeline {
    agent any
    
    stages {
        stage('OWASP Dependency-Check Vulnerabilities') {
            steps {
                script {
                    def odcArgs = """
                        -o './'
                        -s './'
                        -f 'ALL'
                        --prettyPrint
                        --project 'Python Project'
                        --scan 'requirements.txt'
                    """.stripIndent()
                    
                    dependencyCheck additionalArguments: odcArgs, odcInstallation: 'OWASP Dependency-Check Vulnerabilities'
                    
                    dependencyCheckPublisher pattern: 'dependency-check-report.xml'
                }
            }
        }
    }
}
