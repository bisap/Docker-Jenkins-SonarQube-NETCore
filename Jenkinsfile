node {

    stage 'Checkout'
        checkout scm    
    stage 'Build & UnitTest'
        sh "docker build -t calculation:B${BUILD_NUMBER} -f calculation-console/Dockerfile ."  
    stage 'TestCoverage'	    
        sh "docker-compose -f docker-compose.yml up --force-recreate" 
        sh "docker-compose -f docker-compose.yml down -v"
    
}
