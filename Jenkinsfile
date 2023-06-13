pipeline {
    agent { label 'agent_1' }
    stages {
        stage('deploy') {
            steps {
                echo 'deploy'
                script {
                    withCredentials([file(credentialsId: 'kubeconfig', variable: 'KUBECONFIG')]) {
                        sh '''
                            export BUILD_NUMBER=$(cat ../build.txt)
                            mv deployment/deployment.yml deployment/deployment.yml.tmp
                            cat deployment/deployment.yml.tmp | envsubst > deployment/deployment.yml
                            rm -f deployment/deployment.yml.tmp
                            kubectl apply -f deployment -n jenkins --kubeconfig=${KUBECONFIG}
                        '''
                    }
                }
            }
        }
    }
}