pipeline {
    agent { 
        label 'alpine'
    }
    stages {
        stage('sast-semgrep') {
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
        stage('dast-zap') {
        agent { label 'alpine' }
            steps {
                // zap install
                sh 'wget https://github.com/zaproxy/zaproxy/releases/download/v2.15.0/ZAP_2.15.0_Linux.tar.gz'
                sh 'tar -xzf ZAP_2.15.0_Linux.tar.gz'
                sh './ZAP_2.15.0/zap.sh -cmd -addonupdate -addoninstall wappalyzer -addoninstall pscanrulesBeta'
                // Scanning
                sh './ZAP_2.15.0/zap.sh -cmd -quickurl https://s410-exam.cyber-ed.space:8084 -quickout ./zap_results.json'
                // Scanning results uploading
                archiveArtifacts artifacts: 'zap_results.json', allowEmptyArchive: true
                            }
        } 
        }
}
