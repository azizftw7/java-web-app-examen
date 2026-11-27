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

    stage('Deploy') {
        sh """
            if docker service inspect java-web-app >/dev/null 2>&1; then
                docker service update --image azizbz/java-web-app:${BUILD_NUMBER} java-web-app
            else
                docker service create --name java-web-app --replicas 2 -p 8090:8080 --update-failure-action rollback azizbz/java-web-app:${BUILD_NUMBER}
            fi
        """
    }
}
