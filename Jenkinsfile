pipeline {
    // agent { label 'dockerhost-label' }


    stages {
        stage('Test') {
            steps {
                    sh 'echo Testing..'
            }
        }
        stage('Docker build and push ') {
            steps {
                    sh '''
                    dotnet publish --os linux --arch x64 /t:PublishContainer -c Release
                    docker tag todoapi:latest localhost:5000/todoapi:${BUILD_NUMBER}
                    docker push localhost:5000/todoapi:${BUILD_NUMBER}
                    '''
            }
        }
        stage('Deploy') {
            steps {
                    input message: 'Confirm to deploy new app version? (Click "Proceed" to continue)'
                    sh '''
                    docker rm -f todoapi
                    docker run -d --name todoapi -p 9000:8080 localhost:5000/todoapi:${BUILD_NUMBER}
                    '''
            }
        }
    }
    post {
        success {
            echo 'echo "Web APP available on http://192.168.56.11:9000/WeatherForecast"'
        }
    }
}
