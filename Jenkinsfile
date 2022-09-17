node {
    withDockerContainer(args: '-v /root/.m2:/root/.m2', image: 'maven:3.8.1-adoptopenjdk-11') {
        stage('Build') {
            sh 'mvn -B -DskipTests clean package' 
        }
        stage('Test') {
            try {
                sh 'mvn test'
            } catch(e) {
                echo 'Error'
            } finally {
                junit 'target/surefire-reports/*.xml'
            }
        }
        stage('Manual Approval') {
            try {
                input(message: 'Lanjutkan ke tahap Deploy?')
                echo 'Next To Deploy Stage'
            } catch (e) {
                echo 'Approval denied'
                return
            }
        }
        stage('Deploy') {
            sh './jenkins/scripts/deliver.sh'
            sleep 60
        }
    }
}