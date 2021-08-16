pipeline {
    agent any
    triggers { 
        cron('H 2 * * 7')
    }
    stages {
        stage('Clone') {
            steps {
               git url: 'https://github.com/kart-valery/as_code.git',
               credentialsId: "git_hub_kart_ascode"
            }
        }
            stage('Test') {
            steps {
                sh """
                nmap -sn 192.168.202.0/24 > speed.txt
                speedtest --accept-license >> speed.txt
                """
            }
        }
        stage('Push') {
            steps {
                sh """
                git add speed.txt
                git config --global user.email "ascode@test.ru"
                git config --global user.name "As Code"
                git commit -m "Speed_test_as_code"
                git push git@github.com:kart-valery/as_code.git
                """
                
            }
        }
    }
    post {
        always {
            archiveArtifacts artifacts: 'speed.txt', onlyIfSuccessful: true
            deleteDir()
        }
    }
}
