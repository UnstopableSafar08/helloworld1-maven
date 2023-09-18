pipeline {
    agent any
    tools {
        jdk 'jdk11'
        maven 'maven3'
    }
    
    //env global declare 
    environment {
        WAR_ENV='Producion'
    }
    stages {
        stage('Git Checkout') {
            steps {
                git changelog: false, credentialsId: 'c064b52f-854f-49f4-b4fa-d8dee7130326', poll: false, url: 'https://github.com/UnstopableSafar08/helloworld1.git'
            }
        }
        
        stage('COMPILE') {
            steps {
                sh "mvn -B -DskipTests clean compile"
            }
        }
        stage('OWASP DP-Check') {
            steps {
                dependencyCheck additionalArguments: '--scan ./', odcInstallation: 'DP'
                dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }
        stage('BUILD') {
            //it is a local env_var, that overide the global scope env_variable
            environment {
                WAR_ENV='Staging' 
            }
            steps {
                echo WAR_ENV    //echo the env_var
                withCredentials([string(credentialsId: 'd35d05c0-04ab-41b2-b48b-d2351cd4f9d4', variable: 'SEC_VAR')]) {
                    // some block
                    echo SEC_VAR
            }
                sh "mvn -B -DskipTests clean package"
            }
        }
        stage('SAVE-Artiface') {
            steps {
                archiveArtifacts artifacts: '**', followSymlinks: false
            }
        }
    }
}

