pipeline {
    agent any
    triggers{
        pollSCM('*/1 * * * *')
    }
    stages {
        stage('build-docker-image') {
            steps {
                builDockerImage()
            }
        }
        stage('deploy-to-dev') {
            steps {
                deploy("DEV")
            }
        }
        stage('tests-on-dev') {
            steps {
                test("DEV")
            }
        }
        stage('deploy-to-stg') {
            steps {
                deploy("STG")
            }
        }
        stage('tests-on-stg') {
            steps {
                test("STG")
            }
        }
        stage('deploy-to-prod') {
            steps {
                deploy("PROD")
            }
        }
        stage('tests-on-prod') {
            steps {
                test("PROD")
            }
        }
    }
}

def builDockerImage(){
    echo "Building of THE application is starting.."
    sh "docker build -t aceskaemilija/python-greetings-app:latest ."

    echo "Pushing image to docker registry.."
    sh "docker push aceskaemilija/python-greetings-app:latest"
}

def deploy(String environment){
    echo "Deploying Python microservice to ${environment} environment.."
    sh "docker pull aceskaemilija/python-greetings-app"
    sh "docker compose stop greetings-app-${environment.toLowerCase()}"
    sh "docker compose rm greetings-app-${environment.toLowerCase()}"
    sh "docker compose up -d greetings-app-${environment.toLowerCase()}"
}

def test(String environment){
    echo "API test executuon against node application on ${environment} environment.."
    sh "docker pull aceskaemilija/api-tests:latest"
    sh "docker run --network=host --rm aceskaemilija/api-tests:latest run greetings greetings_${environment.toLowerCase()}"
}