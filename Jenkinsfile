pipeline {

agent any

environment {

NEXUS_URL="13.206.222.52:8081"

GROUP_ID="com.example"

ARTIFACT_ID="java-app-cicd"

VERSION="1.0"

DEPLOY_IP="13.127.8.29"

}

stages {

stage('Checkout') {

steps {

git branch: 'main',
url:'https://github.com/USERNAME/java-app-cicd.git'

}

}

stage('SonarQube Analysis') {

steps {

withSonarQubeEnv('sonar-server') {

sh '''

mvn sonar:sonar \
-Dsonar.projectKey=java-app-cicd \
-Dsonar.host.url=http://13.206.222.52:9000

'''

}

}

}

stage('Build') {

steps {

sh 'mvn clean package'

}

}

stage('Upload To Nexus') {

steps {

sh '''

curl -v -u admin:admin123 --upload-file target/java-app-cicd.war \

http://13.206.222.52:8081/repository/maven-releases/com/example/java-app-cicd/1.0/java-app-cicd.war

'''

}

}

stage('Deploy EC2') {

steps {

sh '''

scp target/java-app-cicd.war ubuntu@13.127.8.29:/tmp/

ssh ubuntu@13.127.8.29 << EOF

sudo cp /tmp/java-app-cicd.war /opt/tomcat/webapps/

EOF

'''

}

}

}

}
