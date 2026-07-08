pipeline {
    agent any
    environment{
        IMAGE="fastapi-cicd-app"
        CONTAINER="fastapi-cicd"
        PORT="8000"
        NETWORK="jd_network"
    }
    stages{
        stage("Build Image"){
            steps{
                sh "docker build -t ${IMAGE} ."
            }
        }
        


        stage("Stop and Remove Container"){
            steps{
                sh """
                if docker ps -a --format '{{.Names}}' | grep -w ${CONTAINER}
                then
                docker rm -f ${CONTAINER}
                fi
                """
            }
        }


        stage("Run Container"){
            steps{
                sh "docker run -d --network ${NETWORK} --name ${CONTAINER} -p ${PORT}:${PORT} ${IMAGE}"
            }
        }

        stage("Health check"){
            steps{
                sh """
                sleep 5
                curl http://${CONTAINER}:${PORT}/health
                """
            }
        }
    }
}
