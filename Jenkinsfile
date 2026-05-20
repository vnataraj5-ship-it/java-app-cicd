pipeline {

agent any

tools {

maven 'maven'

}

environment {

SONAR_URL="http://13.206.222.52:9000"

NEXUS_URL="http://13.206.222.52:8081"

DEPLOY_IP="13.127.8.29"

}

stages {

stage('Checkout') {

steps {

git branch:'main',

url:'https://github.com/vnataraj5-ship-it/java-app-cicd.git'

}

}

stage('SonarQube Analysis') {

steps {

withSonarQubeEnv('sonar-server') {

sh '''

mvn clean verify sonar:sonar \

-Dsonar.projectKey=java-app-cicd \

-Dsonar.host.url=$SONAR_URL

'''

}

}

}

stage('Build') {

steps {

sh 'mvn clean package'

}

}

stage('Push Artifact To Nexus') {

steps {

sh '''

curl -u admin:admin123 \

--upload-file target/java-app-cicd.war \

$NEXUS_URL/repository/maven-releases/com/example/java-app-cicd/1.0/java-app-cicd.war

'''

}

}

stage('Deploy EC2') {

steps {

sh '''

ssh ubuntu@13.127.8.29 "

wget -O /tmp/java-app-cicd.war \
http://13.206.222.52:8081/repository/maven-releases/com/example/java-app-cicd/1.0/java-app-cicd.war

sudo cp /tmp/java-app-cicd.war /opt/tomcat/webapps/


"

'''

}

}

}

}
