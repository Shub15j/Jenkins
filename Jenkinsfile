pipeline {
    agent any
    stages {
        stage('Build Maven') {
            steps {
                sh 'pwd'
                sh 'mvn clean install package'
            }
        }
    stage ('Copy Artifacts') {
            steps {
                sh 'pwd'
                sh 'cp -r target/*.jar docker'
            }
        }
    stage('Unit Tests') {
            steps {
                sh 'mvn test'
            }
        }
    stage('Build Docker Image'){
            steps{
                script {
                    def customImage = docker.build("shubhamjadhav1715/petclinic:${env.BUILD_NUMBER}", "./docker")
                    docker.withRegistry('https://registry.hub.docker.com', 'docker-cred') {
                    customImage.push()    
                }
            }
        }
    }
    stage('Build on kubernetes'){
        steps {
            withKubeConfig([credentialsId: 'kube-cred']) {
                sh 'pwd'
                sh 'cp -R helm/* .'
                sh 'ls -ltrh'
                sh 'pwd'
                sh '/usr/local/bin/helm upgrade --install petclinic-app petclinic --set image.repository=shubhamjadhav1715/petclinic --set image.tag=${BUILD_NUMBER}'
        }
    }
}
     

}

}
