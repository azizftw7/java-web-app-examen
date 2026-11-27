node {
    stage('SCM Checkout') {
        git branch: 'master', url: 'https://github.com/azizftw7/java-web-app-examen.git'
    }

    stage('Maven Clean Package') {
        def mvnHome = tool name: 'Maven-3.5.6', type: 'maven'
        sh "${mvnHome}/bin/mvn clean package"
        sh "ls -l target/*.war"
    }
}
