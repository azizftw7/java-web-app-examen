node {
    stage('SCM Checkout') {
        git branch: 'master', url: 'https://github.com/azizftw7/java-web-app-examen.git'
    }

    stage('Maven Clean Package') {
        def mvnHome = tool name: 'Maven-3.5.6', type: 'maven'
        sh "${mvnHome}/bin/mvn clean package"
        sh "ls -l target/*.war"
    }

    stage('Build Docker Image') {
        sh "docker build -t azizbz/java-web-app:${BUILD_NUMBER} ."
        sh "docker tag azizbz/java-web-app:${BUILD_NUMBER} azizbz/java-web-app:latest"
        sh "docker images | grep java-web-app"
    }

    stage('Push Docker Image') {
        withCredentials([string(credentialsId: 'docker_hub_pwd', variable: 'DOCKER_PASS')]) {
            sh 'echo "$DOCKER_PASS" | docker login -u azizbz --password-stdin'
            sh "docker push azizbz/java-web-app:${BUILD_NUMBER}"
            sh "docker push azizbz/java-web-app:latest"
            sh 'docker logout'
        }
    }
}
