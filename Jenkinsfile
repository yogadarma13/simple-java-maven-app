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
        stage('Deploy') {
            sh './jenkins/scripts/deliver.sh'
            sleep 60
        }
    }
}