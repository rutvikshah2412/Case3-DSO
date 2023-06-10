pipeline {
    agent any
    tools{
        jdk 'jdk11'
        maven 'case2-maven'
    }
    environment{
        SCANNER_HOME = tool "sonar-scanner"
    }
    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/rutvikshah2412/Case2-DSO.git'
            }
        }
        stage('Compile') {
            steps {
               sh "mvn clean compile"
            }
        }
        stage('OWASP Check') {
            steps {
                    dependencyCheck additionalArguments: '', odcInstallation: 'DP-check'
                    dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }
        stage('SAST') {
            steps {
              withSonarQubeEnv("sonarqubeServer"){
                sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Case3-DSO \
                -Dsonar.java.binaries=. \
                -Dsonar.projectKey=Case3-DSO '''
              }
//                 withSonarQubeEnv("sonarqubeServer")
            }
        }
        stage('Build') {
            steps {
               sh "mvn clean package -DskipTests=true"
            }
        }
    }
}
