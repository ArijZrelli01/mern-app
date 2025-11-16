pipeline {
    agent any
    triggers { pollSCM('H/5 * * * *') }

    environment {
        IMAGE_SERVER = 'arijzrelli/mern-server'
        IMAGE_CLIENT = 'arijzrelli/mern-client'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                url: 'https://github.com/ArijZrelli01/mern-app.git',
                credentialsId: 'github_ssh'
            }
        }

        stage('Build+Push SERVER') {
            when { changeset 'server/**' }
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',
                    usernameVariable: 'DH_USER',
                    passwordVariable: 'DH_PASS'
                )]) {
                    sh '''
                    echo "$DH_PASS" | docker login -u "$DH_USER" --password-stdin
                    docker build -t $IMAGE_SERVER:${BUILD_NUMBER} server
                    docker push $IMAGE_SERVER:${BUILD_NUMBER}
                    '''
                }
            }
        }

        stage('Build+Push CLIENT') {
            when { changeset 'client/**' }
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',
                    usernameVariable: 'DH_USER',
                    passwordVariable: 'DH_PASS'
                )]) {
                    sh '''
                    echo "$DH_PASS" | docker login -u "$DH_USER" --password-stdin
                    docker build -t $IMAGE_CLIENT:${BUILD_NUMBER} client
                    docker push $IMAGE_CLIENT:${BUILD_NUMBER}
                    '''
                }
            }
			
        }
		
		stage('Security Scan') {
             steps {
              sh '''
             docker run --rm -v /var/run/docker.sock:/var/run/docker.sock \\
             aquasec/trivy image arijzrelli/mern-server:${BUILD_NUMBER} > trivy_report.txt
             echo "=== RAPPORT TRIVY COMPLET ==="
            cat trivy_report.txt
            '''
           }
       }
    }
    post {
        always {
            sh 'docker system prune -af || true'
        }
    }
}