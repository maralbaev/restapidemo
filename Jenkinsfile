node {
  def dockerHubRepo = 'muratbek7@gmail.com/muratbek7'
  def dockerHubCredentialsId = 'DockerHUB_ID'

  stage("Clone the project") {
    git branch: 'main', url: 'https://github.com/maralbaev/restapidemo.git'
  }

  stage("Compilation") {
    sh "chmod +x ./gradlew"
    sh "./gradlew clean build -x test"
  }

  stage("Tests and Deployment") {
    stage("Running unit tests") {
      sh "chmod +x ./gradlew"
      sh "./gradlew test"
    }
    stage("Deployment") {
      sh "chmod +x ./gradlew"
      sh 'nohup ./gradlew bootRun -Dserver.port=8080 &'
      sh 'echo #!asdf2580 | sudo docker login -u muratbek7 --password-stdin'
      sh 'sudo docker build -t muratbek7/restapidemo:1.0 .'
    }
    stage('Push Docker Image') {
        script {
            docker.withRegistry('https://index.docker.io/v1/', dockerHubCredentialsId) {
                def image = docker.image("maralbaev/restapidemo:1.0")
                image.push()
                image.push('1.0')
            }
        }
    }
  }
}
