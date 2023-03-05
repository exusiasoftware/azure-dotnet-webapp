pipeline {
    agent any
    stages {                   
        stage('Build .Net Core') {
            steps {
              sh "dotnet restore"
              sh "dotnet publish"    
            }
        }
        
        stage("Archive Build") {
            steps {
                dir('bin/Release/') {
                   sh "pwd"
                   sh "zip -r archive-${env.BUILD_ID}.zip net7.0"
                   archiveArtifacts artifacts: "archive-${env.BUILD_ID}.zip", followSymlinks: false
                }
            }
        }

        stage('Deploy Azure WebApp') {
          steps {
            dir('bin/Release/') {
             //TestLogin
               withCredentials([usernamePassword(credentialsId: '2482140b-1d99-4098-a788-8ed3c4f6b714', passwordVariable: 'secret', usernameVariable: 'clienntid')]) {
                 sh "az login --service-principal --username $clienntid --tenant ff942d3f-5dc7-4f7b-bad2-99a174ecea1c --password $secret"
                 sh "az webapp deployment source config-zip --resource-group webapp-rg --name testweb-rah --src archive-${env.BUILD_ID}.zip"    
               }
            }
          } 
        }     
    }
}    

