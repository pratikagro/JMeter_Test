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
                "%JMETER_HOME%\\bin\\jmeter.bat" -n ^
                -t "${params.SCRIPT_NAME}" ^
                -l results.jtl ^
                -e ^
                -o report
                """
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
