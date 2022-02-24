pipeline {
    agent { label 'mrcc-linux-test' }
    stages {
        stage('Checkout Codebase'){
            steps{
                //cleanWs() -> Not available on current version
                checkout scm: [$class: 'GitSCM', branches: [[name: '*/main']],userRemoteConfigs:
                [[credentialsId: 'github-ssh-key', url: 'git@github.com:Gvex95/junit-automation.git']]]
            }
        }

        stage('Build'){
            steps{
                // Export javac
                sh 'export PATH="/home/commtester/bin:$PATH"'
                sh 'mkdir lib'
                sh 'cd lib/ ; wget https://repo1.maven.org/maven2/org/junit/platform/junit-platform-console-standalone/1.8.2/junit-platform-console-standalone-1.8.2.jar; cd ..'
                sh 'cd src/ ; javac -cp ../lib/junit-platform-console-standalone-1.8.2.jar *.java; cd ..'
            }
        }

        stage('Test'){
            steps{
                sh 'cd src/ ; java -jar ../lib/junit-platform-console-standalone-1.8.2.jar -cp . --select-class CarTest --reports-dir="reports"'
                junit 'src/reports/*-jupiter.xml'
            }
        }

        stage('Deploy'){
            steps{
                sh 'cd src/ ; java App' 
            }
        }
    }

}