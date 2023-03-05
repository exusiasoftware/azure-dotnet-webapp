pipeline {
    agent any
    environment{
        TENANT = 'ff942d3f-5dc7-4f7b-bad2-99a174ecea1c' 
        RESOURCEGROUP = 'webapp-rg'
        WEBAPPNAME  = 'testweb-rah'
    }
    stages {                   
        stage('Build .Net Core') {
            steps {
              sh "dotnet restore"
              sh "dotnet publish --configuration Release"    
            }
        }
        
        stage("Archive Build") {
            steps {
                dir('bin/Release/net7.0/publish/') {
                   sh "pwd"
                   sh "zip -r archive-${env.BUILD_ID}.zip *"
                   archiveArtifacts artifacts: "archive-${env.BUILD_ID}.zip", followSymlinks: false
                }
            }
        }

        stage('Deploy Azure WebApp Staging Slot') {
          steps {
            dir('bin/Release/net7.0/publish/') {
             //TestLogin
               withCredentials([usernamePassword(credentialsId: '2482140b-1d99-4098-a788-8ed3c4f6b714', passwordVariable: 'secret', usernameVariable: 'clienntid')]) {
                 sh "az login --service-principal --username $clienntid --tenant ${env.TENANT} --password $secret"
                 sh "az webapp deployment source config-zip --slot staging --resource-group ${env.RESOURCEGROUP} --name ${env.WEBAPPNAME}--src archive-${env.BUILD_ID}.zip"  
 
               }
            }
          } 
        }    
        
       stage("Approval") {
            steps {
               input 'Approval Stage'
            }
        }

        stage('Swap Azure WebApp Staging Production') {
          steps {
             //TestLogin
               withCredentials([usernamePassword(credentialsId: '2482140b-1d99-4098-a788-8ed3c4f6b714', passwordVariable: 'secret', usernameVariable: 'clienntid')]) {
                 sh "az login --service-principal --username $clienntid --tenant ${env.TENANT} --password $secret"
                 sh "az webapp deployment slot swap  -g ${env.RESOURCEGROUP} -n ${env.WEBAPPNAME} --slot staging --target-slot production"  
               }
          }
           
        }  
    }
}    

