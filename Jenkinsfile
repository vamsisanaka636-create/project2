pipeline {
    agent any
    environment {
    PATH = "/usr/local/src/apache-maven/bin:$PATH"
    }
    stages {
        stage('GitHub Clone') {
            steps {
                git branch: 'project2', credentialsId: 'e726a254-2f49-4ec1-bc28-bc30f0da4e33'
            }
        }
        stage('Build Maven') {
            steps {
                sh "mvn clean install package"
            }
        }
        stage('Deploy Tomcat') {
            steps {
                deploy adapters: [tomcat9(alternativeDeploymentContext: '',  credentialsId: 'tomcat',
                path: '',
                url: 'http://15.206.179.253:8080/')],
                contextPath: 'project2',
                war: '**/*.war'
            }
        }
        stage('add files') {
            steps {
                sh 'git add Jenkinsfile'
            }
        }
        
    }
    post {
        success {
            emailext to: "vamsisanaka636@gmail.com",
            recipientProviders: [developers()],
            subject: "jenkins Pipe :${currentBuild.currentResult}: ${env.JOB_NAME}",
            body: "${currentBuild.currentResult}: Job ${env.JOB_NAME}\n More Info can be found here: ${env.BUILD_URL}",

            attachLog: true

            slackSend message: "Build deployed successfully - Job ${env.JOB_NAME}\n More Info can be found here: ${env.BUILD_URL}"
        }
    }
}
