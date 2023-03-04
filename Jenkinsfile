pipeline {
    agent any
    stages {                   
        stage('Build .Net Core') {
            steps {
              sh "pwd"
              sh "dotnet publish --configuration Release"    
            }
        }
    }
}