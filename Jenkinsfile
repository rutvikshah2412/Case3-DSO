pipeline {
    agent any
    tools{
        jdk 'jdk11'
        maven 'case2-maven'
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
        stage('Build') {
            steps {
               sh "mvn clean package -DskipTests=true"
            }
        }
    }
}
