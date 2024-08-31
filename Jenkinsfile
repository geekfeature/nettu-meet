pipeline {
    agent { 
        label 'alpine'
    }
    stages {
        stage('Static-semgrep') {
            steps {
                script {
                // Semgrep install
                sh 'apk add python3'
                sh 'apk add --update pipx'
                sh 'pipx install semgrep; pipx ensurepath; source ~/.bashrc'
                // Scanning
                sh '/root/.local/bin/semgrep scan --config auto --verbose --json > semgrep_results.json'
                // Scanning results uploading
                archiveArtifacts artifacts: 'semgrep_results.json', fingerprint: true 
            }
            }
        }
        }
}
