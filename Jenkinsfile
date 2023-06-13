pipeline {
    agent any
    stages {
        stage('build') {
            steps {
                echo 'build'
                script{
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-acc', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                        sh '''
                            docker login -u ${USERNAME} -p ${PASSWORD}
                            docker build --network=host -t ahmedlsheriff/laravel-app:v${BUILD_NUMBER} . --
                            docker push ahmedlsheriff/laravel-app:v${BUILD_NUMBER}
                            echo ${BUILD_NUMBER} > ../build.txt
                        '''
                    }
                }
            }
        }
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
                            kubectl apply -f deployment -n default --kubeconfig=${KUBECONFIG}
                        '''
                    }
                }
            }
        }
    }
}