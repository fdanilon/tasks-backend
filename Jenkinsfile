pipeline {
    agent any
    stages {
        stage ('Build backend') {
            steps {
                bat 'mvn clean package -DskipTests=true'
            }
        }
        stage ('Unit tests') {
            steps {
                bat 'mvn test'
            }
        }
        // stage ('Sonar analysis') {
        //     environment {
        //         scannerHome = tool 'SONAR_SCANNER'
        //     }
        //     steps {
        //         withSonarQubeEnv('SONAR_LOCAL'){
        //             bat "${scannerHome}/bin/sonar-scanner -e -Dsonar.projectKey=DeployBack -Dsonar.host.url=http://localhost:9000 -Dsonar.login=a8a9e216c17c9f2de4bdda80da62f6022213f2d8 -Dsonar.java.binaries=target -Dsonar.coverage.exclusions=**/.mvn/**, **/src/test/**, **/model/**, **Application.java"
        //         }                
        //     }
        // }
        // stage ('Quality Gate') {
        //     steps {
        //         timeout(time: 1, unit: 'MINUTES')
        //         waitForQualityGate abortPipeline: true
        //     }
        // }
        stage ('Deploy Backend'){
            steps {
                deploy adapters: [tomcat8(credentialsId: 'TomcatLogin', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks-backend', war: 'target/tasks-backend.war'
            }
        }
        stage ('API Test'){
            steps {
                dir('api-test'){
                    git branch: 'main', credentialsId: 'github_login', url: 'https://github.com/fdanilon/tasks-api-test'
                    bat 'mvn test'
                }
            }
        }
    }
}

