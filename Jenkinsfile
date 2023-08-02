
node{
  def mavenHome = tool name: 'maven'
  stage('1cloneCode'){
    git branch: 'main', url: 'https://github.com/Mikey-06/Coffee_Club_Reg_App.git'
  }
  stage('2Test&Build'){
    sh "${mavenHome}/bin/mvn clean package"
  }

  def app

  stage('Build image') {
  
       app = docker.build("mikey6/coffe-club-reg-app")
  }

  stage('Push image') {
        
        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            app.push("${env.BUILD_NUMBER}")
        }
    }

  stage('Trigger ManifestUpdate') {
        echo "triggering Coffe_Club_Reg_App(Update-Manifest)"
        build job: 'Coffe_Club_Reg_App(Update-Manifest)', parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]
    }

}
