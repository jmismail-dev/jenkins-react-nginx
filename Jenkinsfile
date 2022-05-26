pipeline {
    agent any
    tools { nodejs 'Node16.15.0' }
    stages {    
        stage('Check') {
            steps {
                sh '''
                    npm --version
                    node --version
                '''
            }
        }
        stage('Build') {
            steps {
                sh '''
                   npm install
                   npm run build
                '''
            }
        }
        stage('Deploy') {
            steps {
                withCredentials([
                    string(credentialsId: 'ts-notes-server-ip', variable: 'SERVER_IP_ADDRESS'),
                    string(credentialsId: 'ts-notes-server-password', variable: 'SERVER_PASSWORD'),
                ]) {
                    sh "cd ${WORKSPACE}"
                    // sh 'sudo rsync -avr -e "ssh -l jmismail" --exclude="client" . jmismail@${SERVER_IP_ADDRESS}:/home/jmismail/jenkins-react-nginx'
                    sh 'sudo rsync -avz --stats --rsync-path="echo ${SERVER_PASSWORD} | sudo -Sv && sudo rsync" ${WORKSPACE}/dist/ jmismail@${SERVER_IP_ADDRESS}:/home/jenkins-react-nginx/'
                }
            }
        }
    }
}
