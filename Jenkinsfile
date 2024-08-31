pipeline {
    agent { 
        label 'alpine'
    }
    stages {
/*          stage('sast-semgrep') {
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
            agent {
                label 'dind'
            }
            steps {
                script {
                    sh 'wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | sudo apt-key add -'
                    sh 'echo deb https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main | sudo tee -a /etc/apt/sources.list.d/trivy.list'
                    sh 'sudo apt-get update'
                    sh 'sudo apt-get install trivy'
                    sh 'trivy repo https://github.com/geekfeature/nettu-meet -f json -o trivy_result.json'
                    archiveArtifacts artifacts: 'trivy_result.json', allowEmptyArchive: true
                }   
            }
                                            
        } */

	    stage('syft'){
            steps {
                script{
                    sh 'curl -sSfL https://raw.githubusercontent.com/anchore/syft/main/install.sh | sh -s -- -b /usr/local/bin'
                    sh 'syft dir:$(pwd) -o cyclonedx-json > sbom.json'
			        archiveArtifacts artifacts: 'sbom.json', allowEmptyArchive: true	
}
}