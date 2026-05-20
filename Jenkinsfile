pipeline {

agent any

tools {
    maven 'maven'
}

environment {

    SONAR_URL = "http://13.206.222.52:9000"

    NEXUS_URL = "http://13.206.222.52:8081"

    DEPLOY_IP = "13.127.8.29"

    ARTIFACT_NAME = "java-app-cicd-1.0.war"

}

stages {

    stage('Checkout') {

        steps {

            git branch: 'main',
            url: 'https://github.com/vnataraj5-ship-it/java-app-cicd.git'

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

            sh '''
            mvn clean package
            '''

        }

    }

    stage('Push Artifact To Nexus') {

        steps {

            sh '''
            curl -v -u admin:Qwert@mnv1234 \
            --upload-file target/$ARTIFACT_NAME \
            $NEXUS_URL/repository/maven-releases/com/example/java-app-cicd/1.0/$ARTIFACT_NAME
            '''

        }

    }

    stage('Deploy To EC2') {

        steps {

            sh '''
            ssh -o StrictHostKeyChecking=no ubuntu@$DEPLOY_IP "

            wget \
            --user=admin \
            --password='admin123' \
            -O /tmp/$ARTIFACT_NAME \
            $NEXUS_URL/repository/maven-releases/com/example/java-app-cicd/1.0/$ARTIFACT_NAME

            sudo cp /tmp/$ARTIFACT_NAME /var/lib/tomcat10/webapps/

            sudo systemctl restart tomcat10

            "
            '''

        }

    }

}

post {

    success {

        echo 'PIPELINE SUCCESS'

    }

    failure {

        echo 'PIPELINE FAILED'

    }

}

}
