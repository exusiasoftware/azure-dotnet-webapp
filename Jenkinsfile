pipeline {
    agent any
    stages {                   
        stage('Build .Net Core') {
            steps {
              sh "pwd"
              sh "dotnet publish --configuration Release"    
            }
        }
        
        stage("Archive Build") {
            steps {
                dir('bin/Release/net7.0/') {
                   sh "pwd"
                   sh "zip -r archive-${env.BUILD_ID}.zip publish"
                   archiveArtifacts artifacts: "archive-${env.BUILD_ID}.zip", followSymlinks: false
                }
            }
        }

        stage('Deploy Azure WebApp') {
          steps {
          //TestLogin
            withCredentials([usernamePassword(credentialsId: '2482140b-1d99-4098-a788-8ed3c4f6b714', passwordVariable: 'secret', usernameVariable: 'clienntid')]) {
               sh "az login --service-principal --username $clienntid --tenant ff942d3f-5dc7-4f7b-bad2-99a174ecea1c --password $secret"
              // sh "az webapp deployment source config-zip --resource-group myResourceGroup --name myWebApp --src zipFilePathLocation"    
            }
          } 
        }     
    }
}    

