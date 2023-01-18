pipeline  {
    agent any
    tools {
    maven 'apache-maven-3.6.3'}
        
    stages {

        stage('Build') {
            steps{
            sh "mvn clean package"
        }
        }
        

        stage('Publish') {
            steps{
            // login Azure
            withCredentials([
                            string(credentialsId: 'AZ_USER', variable: 'azuser'),
                            string(credentialsId: 'AZ_PASS', variable: 'azpass'),
                            string(credentialsId: 'AZ_TENANT', variable: 'aztenant'), 
                            ]) {
            sh '''
                sh 'az login --service-principal --username $azuser --password $azpass --tenant $aztenant'
                
            '''
            }
            sh 'cd $PWD/target/azure-functions/odd-or-even-function-sample && zip -r ../../../archive.zip ./* && cd -'
            sh "az functionapp deployment source config-zip -g 'az_functions' -n 'javateste' --src archive.zip"
            sh 'az logout'
        }
        }
    }
}
