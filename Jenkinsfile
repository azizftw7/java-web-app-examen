node {
    stage('SCM Checkout') {
        git branch: 'master', url: 'https://github.com/azizftw7/java-web-app-examen.git'
    }

    stage('Maven Clean Package') {
        def mvnHome = tool name: 'Maven-3.5.6', type: 'maven'
        sh "${mvnHome}/bin/mvn clean package"
        sh "ls -l target/*.war"
    }

    stage('Docker Build') {
        sh "docker build -t azizbz/java-web-app:latest ."
        sh "docker images | grep java-web-app"
    }

    stage('Docker Hub Push') {
        withCredentials([usernamePassword(credentialsId: 'docker_hub_pwd', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
            sh 'echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin'
            sh 'docker push azizbz/java-web-app:latest'
            sh 'docker logout'
        }
    }
}
