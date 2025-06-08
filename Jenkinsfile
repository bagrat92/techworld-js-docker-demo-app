pipeline{
    options {
        buildDiscarder(logRotator(numToKeepStr: '5'))
    }
    agent any

    environment {
        registry = "bagrat92/myapp"
    }

    stages{
        stage ('Pull Code from GitHub'){
            steps{
                script {
                    git branch: 'master',
                        credentialsId: 'github-ssh-key',
                        url: 'git@github.com:bagrat92/techworld-js-docker-demo-app.git'
                }
            }
        }
        stage('Building image'){
            steps{
                sh '''
                  docker build -t ${registry}:${BUILD_ID} .
                '''
            }
        }
        stage('Docker Push'){
            steps {
                withCredentials(
                    [usernamePassword
                        (credentialsId: 'docker-cred', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
                        sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
                        sh 'docker push ${registry}:${BUILD_ID}'
                }
            }
        }
        stage('Deploy to EKS'){
            steps{
              sh '''
              pwd
              ls -la
              '''
            }
        }
    }
}