pipeline {
    agent any

    environment {
        JMETER_HOME = 'C:\\apache-jmeter-5.6.3\\apache-jmeter-5.6.3'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Run JMeter Test') {
            steps {
                bat """
				if exist report rmdir /s /q report
				if exist results.jtl del /f /q results.jtl
				
                "%JMETER_HOME%\\bin\\jmeter.bat" -n ^
                -t "${params.SCRIPT_NAME}" ^
                -l results.jtl ^
                -e ^
                -o report
                """
            }
        }
		
		stage('Publish JMeter Report') {
            steps {
                publishHTML(target: [
                    allowMissing: false,
                    alwaysLinkToLastBuild: true,
                    keepAll: true,
                    reportDir: 'report',
                    reportFiles: 'index.html',
                    reportName: 'JMeter HTML Report'
                ])
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'results.jtl', fingerprint: true
            archiveArtifacts artifacts: 'report/**', fingerprint: true
        }
    }
}
