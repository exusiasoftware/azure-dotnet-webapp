pipeline {
    agent any
    stages {
        
        stage('Check in SCM') {
            steps {
                git url: 'https://exusiasoftware@bitbucket.org/exusiasoftware/insertmanager.git'
                echo 'Download Complete...'
            }
        }
                   
        stage('Build .Net Core') {
            steps {
              sh "pwd"
              sh "dotnet publish --configuration Release"    
            }
        }
        
        stage('Docker Build') {
            steps {
                echo 'Building Docker file'
                sh "pwd"
                sh "docker build . -t insertmgr:${env.BUILD_ID} -f Docker/Dockerfile_API "
            }
        }
        

        stage("Approval") {
            steps {
               input 'Approval Stage'
            }
        }
        

        stage("Archive Build") {
            steps {
                dir('API/bin/Release/net5.0') {
                   sh "pwd"
                   sh "zip -r archive-${env.BUILD_ID}.zip publish"
                   archiveArtifacts artifacts: "archive-${env.BUILD_ID}.zip", followSymlinks: false
                }
            }
        }
    }
}