pipeline {
    agent any
     triggers {
        githubPush()
      }
    stages {
        stage('Restore packages'){
           steps{
               sh 'dotnet restore WebApplication.sln'
            }
         }        
        stage('Clean'){
           steps{
               sh 'dotnet clean WebApplication.sln --configuration Release'
            }
         }
        stage('Build'){
           steps{
               sh 'dotnet build WebApplication.sln --configuration Release --no-restore'
            }
         }
        stage('Test: Unit Test'){
           steps {
                sh 'dotnet test XUnitTestProject/XUnitTestProject.csproj --configuration Release --no-restore'
             }
          }
        stage('Publish'){
             steps{
               sh 'dotnet publish WebApplication/WebApplication.csproj --configuration Release --no-restore'
             }
        }
        stage('Deploy'){
             steps{
               sh '''for pid in $(lsof -t -i:9090); do
                       kill -9 $pid
               done'''
                 
               sh 'rm -d  archive'
               sh 'cd WebApplication/bin/Release/netcoreapp3.1/publish/'
               sh 'mkdir archive'
               sh 'zip -r WebApplication.zip WebApplication/bin/Release/netcoreapp3.1/publish/'
                 
                 
                 withAWS(profile:'jenkins-server-s3-full-access') {
                // Upload files from working directory 'dist' in your project workspace
                sh 'aws s3 cp /var/lib/jenkins/workspace/Test_dev/   s3://my-kash-bucket/  --include "*.zip"'
                }
                 
                 
                 
               
               sh  'pwd'
             }            
        } 
        
      
        
        
    }
}
