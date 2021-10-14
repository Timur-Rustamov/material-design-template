node {
    nodejs = tool 'nodejs'
    properties([pipelineTriggers([githubPush(), pollSCM('0 0 * * *')])])
    stage('Checkout') { 
        git 'https://github.com/AvaTTaR/mdt.git'
    }
    
    stage('JS preparing') {
      sh "cat www/js/* | ${nodejs}/bin/uglifyjs -o www/min/merged-and-compressed.js --compress"
    }
    
    stage('CSS preparing') {
      sh "cat www/css/* | ${nodejs}/bin/cleancss -o www/min/merged-and-minified.css"
    }

    stage('Archiving') {
        sh "tar --exclude=.git --exclude=www/js --exclude=www/css -czvf /tmp/result.tar.gz ."
    }
    stage('Deploy') {
        sh 'curl -uadmin:Password_1 -T /tmp/result.tar.gz http://165.232.70.142:8082/artifactory/pipe/result_v_$BUILD_ID.tar.gz'
    }
}
