pipeline {
    agent any
    tools{
        jdk 'jdk11'
        maven 'case2-maven'
    }
    environment{
        SCANNER_HOME = tool "sonar-scanner"
        DOCKERHUB_CREDENTIALS=credentials('DockerCred')
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
            }
        }
        
        stage('Build') {
            steps {
               sh "mvn clean package -DskipTests=true"
            }
        }
        stage('Docker Build and Push') {
            steps {
                    sh 'docker build -t rutvikshah2412/case3-dso:${BUILD_NUMBER} .'
//                     sh 'docker build -t penclinic .'
//                     sh 'docker tag petclinic rutvikshah2412/case3-dso:${BUILD_NUMBER}'
                    sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                    sh 'docker push rutvikshah2412/case3-dso:${BUILD_NUMBER}'
                
//                withDockerRegistry(credentialsId: 'DockerCred', url: 'https://registry.hub.docker.com') {
//                    sh 'docker build -t rutvikshah2412/case3-dso:${BUILD_NUMBER} .'
//                    sh 'docker push rutvikshah2412/case3-dso:${BUILD_NUMBER}'
//                }
            }
            stage('Trivy') {
            steps {
               sh "trivy image rutvikshah2412/case3:32"
            }
        }
        }
    }
}
