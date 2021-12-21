// node{
//     stage("Check out SCM"){
//         git credentialsId: 'mehedi02', url: 'https://github.com/mehedi02/jenkins_test.git'
//     }
    
//     stage("Build Docker image for App1"){
//         dir('app1') {
//             sh "docker image build -t mehedi02/laravel-k8s-demo-1:jenkins . -f build/Dockerfile"
//         }
//     }
    
//     stage("Build Docker image for App2"){
//         dir('app2') {
//             sh "docker image build -t mehedi02/laravel-k8s-demo-2:jenkins . -f build/Dockerfile"
//         }
//     }
    
//     stage("Push two docker image to dockerhub"){
//         withCredentials([string(credentialsId: 'docker-pwd', variable: 'dockerHub')]) {
//             sh "docker login -u mehedi02 -p ${dockerHub}"
//         }
//         sh "docker push mehedi02/laravel-k8s-demo-1:jenkins"
//         sh "docker push mehedi02/laravel-k8s-demo-2:jenkins"
//     }
// }

pipeline {
    agent any
    stages {
        stage("Checkout SCM"){
            steps {
                git credentialsId: 'mehedi02', url: 'https://github.com/mehedi02/jenkins_test.git'
            }
        }

        stage("Build Docker Image for App1"){
            steps{
                dir('app1') {
                    sh "docker image build -t mehedi02/laravel-k8s-demo-1:jenkins . -f build/Dockerfile"
                }

                dir('app2') {
                    sh "docker image build -t mehedi02/laravel-k8s-demo-2:jenkins . -f build/Dockerfile"
                }
            }
        }

        stage("Push two docker image to dockerhub"){
            steps {
                withCredentials([string(credentialsId: 'docker-pwd', variable: 'dockerHub')]) {
                    sh "docker login -u mehedi02 -p ${dockerHub}"
                }
                sh "docker push mehedi02/laravel-k8s-demo-1:jenkins"
                sh "docker push mehedi02/laravel-k8s-demo-2:jenkins"
            }
        }

        stage("Deploy app") {
            steps {
                dir("infra/config") {
                    script {
                        kubernetesDeploy(configs: "configmap.yaml", kubeconfigId: "mykubernetesconfig")
                    }
                }
                dir("app1/deploy"){
                    script {
                        kubernetesDeploy(configs: "deployment-1.yaml", kubeconfigId: "mykubernetesconfig")
                    }
                    
                    script {
                        kubernetesDeploy(configs: "service-1.yaml", kubeconfigId: "mykubernetesconfig")
                    }
                }
                dir("app2/deploy"){
                    script {
                        kubernetesDeploy(configs: "deployment-2.yaml", kubeconfigId: "mykubernetesconfig")
                    }
                    
                    script {
                        kubernetesDeploy(configs: "service-2.yaml", kubeconfigId: "mykubernetesconfig")
                    }
                }
                dir("nginx/deploy"){
                    script {
                        kubernetesDeploy(configs: "nginx_deployment.yaml", kubeconfigId: "mykubernetesconfig")
                    }
                    
                }
            }
        }
    }
}