node {

    stage 'Checkout'
        checkout scm
    stage 'Build & UnitTest'
        sh "docker build -t calculation:B${BUILD_NUMBER} -f calculation/Dockerfile ."      
    stage 'TestCoverage'
        sh "docker-compose -f  up --force-recreate" 
        sh "docker-compose -f down -v"
}
