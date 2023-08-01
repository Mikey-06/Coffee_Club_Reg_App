pipeline {
    agent any 
    environment{
        DOCKERHUB_CREDS = credentials('dockerhub')
     }     
    tools {
        maven "maven"
  } 

    stages {
        stage('Pull code') {
            steps {
                git branch: 'main', url: 'https://github.com/Mikey-06/Coffee_Club_Reg_App.git'
            }
        }
        
        stage('Build + Test') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Image') {
            steps {
                sh 'docker build -t mikey6/coffe-club-reg-app:$BUILD_NUMBER ./docker/'
            }
        }

        stage('Docker Login') {
            steps {
                 sh 'echo $DOCKERHUB_CREDS_PSW | docker login -u $DOCKERHUB_CREDS_USR --password-stdin'
            }
        }

        stage('Docker Push') {
            steps {
                sh 'docker push mikey6/coffe-club-reg-app:$BUILD_NUMBER'
            }
        }

        stage('Trigger ManifestUpdate') {
                echo "triggering Coffe_Club_Reg_App(Update-Manifest)"
                build job: 'Coffe_Club_Reg_App(Update-Manifest)', parameters: [string(name: 'DOCKERTAG', value: $BUILD_NUMBER)]
        {
            
    }    

    post {
        always {
            sh 'docker logout'
           }
    }
    
}