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
                sh 'pwd'
                sh 'wget https://github.com/zaproxy/zaproxy/releases/download/v2.15.0/ZAP_2.15.0_Linux.tar.gz'
                sh 'tar -xzf ZAP_2.15.0_Linux.tar.gz'
                sh './ZAP_2.15.0/zap.sh -cmd -addonupdate -addoninstall wappalyzer -addoninstall pscanrulesBeta'
                // Scanning
                sh './ZAP_2.15.0/zap.sh -cmd -quickurl https://s410-exam.cyber-ed.space:8084 -quickout $(pwd)/zap_results.json'
                // Scanning results uploading
                stash name: 'zap_results', includes: 'zap_results.json' 
                archiveArtifacts artifacts: 'zap_results.json', allowEmptyArchive: true
                            }
        }
        stage('trivy') {
        agent { label 'dind' }
            steps {
                sh 'docker pull bitnami/trivy'
                sh 'docker run --name trivy bitnami/trivy:latest'
                sh 'docker build -t nettu-meet-server:latest -f ./server/Dockerfile'
                sh 'trivy image --format cyclonedx --output ./sbom_server.json nettu-meet-server:latest'
                sh 'trivy sbom -o ./trivy_server.json ./sbom_server.json'
                sh 'docker build -t nettu-meet-frontend:latest -f ./frontend/docker/Dockerfile'
                sh 'trivy image --format cyclonedx --output ./sbom_frontend.json nettu-meet-frontend:latest'
                sh 'trivy sbom -o ./trivy_frontend.json ./sbom_frontend.json'
              }
              archiveArtifacts artifacts: 'trivy_server.json, trivy_frontend.json, sbom_server.json, sbom_frontend.json', followSymlinks: false
            } 
                                            
        }
}
