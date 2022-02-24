pipeline {
    agent none
    stages {
        stage('Checkout Codebase'){
            agent { label 'mrcc-linux-test' }
            steps{
                //cleanWs() -> Not available on current version
                checkout scm: [$class: 'GitSCM', branches: [[name: '*/main']],userRemoteConfigs:
                [[credentialsId: 'github-ssh-key', url: 'git@github.com:Gvex95/junit-automation.git']]]
            }
        }

        stage('Build'){
            agent { label 'mrcc-linux-test' }
            steps{
                sh 'mkdir lib'
                sh 'cd lib/ ; wget https://repo1.maven.org/maven2/org/junit/platform/junit-platform-console-standalone/1.8.2/junit-platform-console-standalone-1.8.2.jar'
                sh 'cd /home/commtester/bin ; ./javac -cp "/home/commtester/mgvero/junit_test_example/lib/junit-platform-console-standalone-1.8.2.jar" "/home/commtester/mgvero/junit_test_example/src/*.java"
            }
        }

        stage('Test'){
            agent { label 'mrcc-linux-test' }
            steps{
                sh 'cd src/ ; java -jar ../lib/junit-platform-console-standalone-1.8.2.jar -cp "." --select-class CarTest --reports-dir="reports"'
                junit 'src/reports/*-jupiter.xml'
            }
        }

        stage('Deploy'){
            agent { label 'mrcc-linux-test' }
            steps{
                sh 'cd src/ ; java App' 
            }
        }
    }

}