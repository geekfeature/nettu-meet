pipeline {
    agent { 
        label 'alpine'
    }
    stages {
/*           stage('sast-semgrep') {
            steps {
                script {
                // Semgrep install
                sh 'apk add python3'
                sh 'apk add --update pipx'
                sh 'pipx install semgrep; pipx ensurepath; source ~/.bashrc'
                // Scanning
                sh '/root/.local/bin/semgrep scan --config auto --verbose --json > semgrep_results.json'
                // Scanning results uploading
                sh 'curl -X "POST" -kL "https://s410-exam.cyber-ed.space:8083/api/v2/import-scan/" -H "accept: application/json" -H "Authorization: Token c5b50032ffd2e0aa02e2ff56ac23f0e350af75b4" -H "Content-Type: multipart/form-data" -F "active=true" -F "verified=true" -F "deduplication_on_engagement=true" -F "minimum_severity=High" -F "scan_date=2024-08-31" -F "engagement_end_date=2024-08-31" -F "group_by=component_name" -F "tags=" -F "product_name=exam_morev" -F "file=@semgrep_results.json;type=application/json" -F "auto_create_context=true" -F "scan_type=Semgrep JSON Report" -F "engagement=1"'
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
                sh 'curl -X "POST" -kL "https://s410-exam.cyber-ed.space:8083/api/v2/import-scan/" -H "accept: application/json" -H "Authorization: Token c5b50032ffd2e0aa02e2ff56ac23f0e350af75b4" -H "Content-Type: multipart/form-data" -F "active=true" -F "verified=true" -F "deduplication_on_engagement=true" -F "minimum_severity=High" -F "scan_date=2024-08-31" -F "engagement_end_date=2024-08-31" -F "group_by=component_name" -F "tags=" -F "product_name=exam_morev" -F "file=@zap_results.json;type=application/json" -F "auto_create_context=true" -F "scan_type=Semgrep JSON Report" -F "engagement=1"'
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
                    sh 'curl -X "POST" -kL "https://s410-exam.cyber-ed.space:8083/api/v2/import-scan/" -H "accept: application/json" -H "Authorization: Token c5b50032ffd2e0aa02e2ff56ac23f0e350af75b4" -H "Content-Type: multipart/form-data" -F "active=true" -F "verified=true" -F "deduplication_on_engagement=true" -F "minimum_severity=High" -F "scan_date=2024-08-31" -F "engagement_end_date=2024-08-31" -F "group_by=component_name" -F "tags=" -F "product_name=exam_morev" -F "file=@trivy_result.json;type=application/json" -F "auto_create_context=true" -F "scan_type=Semgrep JSON Report" -F "engagement=1"'
                    archiveArtifacts artifacts: 'trivy_result.json', allowEmptyArchive: true
                }   
            }
                                            
        } 

	    stage('syft'){
            steps {
                script{
                    sh 'curl -sSfL https://raw.githubusercontent.com/anchore/syft/main/install.sh | sh -s -- -b /usr/local/bin'
                    sh 'syft dir:$(pwd) -o cyclonedx-json > sbom.json'
			        archiveArtifacts artifacts: 'sbom.json', allowEmptyArchive: true	
}
}
} */

        stage('QualtityGate') {
            agent {
                label 'alpine'
            }

            steps {
                unstash 'zap_results'
                script {                   
                   sh '''
                      ERR=$(zap_results.json | jq | grep -iE '"riskdesc": "High"' | wc -l)
                     if [ $ERR =! 0 ]; then
                      echo "QualityGate failed";
                        exit 1
                   fi
                      '''
                    }
                }


}
}
