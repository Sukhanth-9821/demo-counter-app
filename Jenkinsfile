pipeline{
    
    agent any 
    
    stages {
        
        stage('Git Checkout'){
            
            steps{
                
                script{
                    
                    git branch: 'main', url: 'https://github.com/Sukhanth-9821/demo-counter-app.git'
                }
            }
        }
        stage('UNIT testing'){
            
            steps{
                
                script{
                    
                    sh 'mvn test'
                }
            }
        }
        stage('Integration testing'){
            
            steps{
                
                script{
                    
                    sh 'mvn verify -DskipUnitTests'
                }
            }
        }
        stage('Maven build'){
            
            steps{
                
                script{
                    
                    sh 'mvn clean install'
                }
            }
        }
        stage('Static code analysis'){
            
            steps{
                
                script{
                    
                    withSonarQubeEnv(credentialsId: 'Sonar') {
                        
                        sh 'mvn clean package sonar:sonar'
                    }
                   }
                    
                }
            }
            
        stage('Quality Gate Status'){
                
                steps{
                    
                    script{
                        
                        waitForQualityGate abortPipeline: false, credentialsId: 'Sonar'
                    }
                }
            } 
        stage('Nexus Upload'){
                
                steps{
                    
                    script{
                        def pom = readMavenPom file: 'pom.xml'
                        def reposelect = pom.version.endsWith("SNAPSHOT")? "demorepo-snapshot":"demorepo"
                        nexusArtifactUploader artifacts: [[artifactId: 'springboot', classifier: '', file: 'target/Uber.jar', type: 'jar']], credentialsId: 'Nexus', groupId: "${pom.groupId}", nexusUrl: '15.207.18.215:8081', nexusVersion: 'nexus3', protocol: 'http', repository: reposelect, version: "${pom.version}"
                    }
                }
            }

    }
     
}