node {
    checkout scm
    stage('Build') {
        docker.image('maven:3.8.6-eclipse-temurin-18-alpine').inside('-v /root/.m2:/root/.m2') {
                sh 'mvn -B -DskipTests clean package'
        }
    }
    stage('Test') {
        try {
                {docker.image('maven:3.8.6-eclipse-temurin-18-alpine').inside('-v /root/.m2:/root/.m2') 
                        sh 'mvn test'
                }
        } catch (e) {
                echo "Test Stage Failed!"
        } finally {
                junit 'target/surefire-reports/*.xml'
        }
    }
    stage('Manual Approval') {
        docker.image('maven:3.8.6-eclipse-temurin-18-alpine').inside('-v /root/.m2:/root/.m2') {
                input message: 'Lanjutkan ke tahap Deploy?'
        }
    }
    stage('Deploy') {
        docker.image('maven:3.8.6-eclipse-temurin-18-alpine').inside('-v /root/.m2:/root/.m2') {
                sh './jenkins/scripts/deliver.sh'
				sleep(time: 1, unit: 'MINUTES')
        }
    }
}