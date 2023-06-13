pipeline {
    agent { label 'agent_1' }
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
                            export release=$(helm list --short | grep ^app)     
                            if [ -z $release ]
                            then
                                helm install app deployment/app \
                                --set BUILD_NUMBER=${BUILD_NUMBER}
                                --kubeconfig=${KUBECONFIG}
                            else
                                helm upgrade app deployment/app \
                                --set BUILD_NUMBER=${BUILD_NUMBER}
                                --kubeconfig=${KUBECONFIG}
                            fi
                        '''
                    }
                }
            }
        }
    }
}