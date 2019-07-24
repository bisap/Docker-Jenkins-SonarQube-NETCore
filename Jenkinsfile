node {

    stage 'Checkout'
        checkout scm       
    stage 'TestCoverage'
        sh "docker-compose -f docker-compose.yml up --force-recreate" 
        sh "docker-compose -f docker-compose.yml down -v"
}
