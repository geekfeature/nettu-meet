pipeline {
    agent { 
        label 'alpine'
    }
    stages {
        stage('Static-semgrep') {
            steps {
                script {
                    //sh 'apk add docker'
                    sh 'apk add python3'
                    sh 'apk add --update pipx'
                    sh 'pipx install semgrep; pipx ensurepath; source ~/.bashrc'
                    sh '/root/.local/bin/semgrep scan --config auto > semgrep.txt'
                    //sh '/root/.local/bin/semgrep ci'
                    archiveArtifacts artifacts: 'semgrep.txt', allowEmptyArchive: true
                }
            }
        }
        }
}
