node ("Slave") {
    nodejs = tool 'nodejs'
    properties([pipelineTriggers([githubPush(), pollSCM('0 0 * * *')])])
    stage('Checkout') { 
        checkout scm
    }
    def preparing = ["JS preparing" : {stage("JS preparing") { sh "cat www/js/* | ${nodejs}/bin/uglifyjs -o www/min/merged-and-compressed.js --compress" }},
                    "CSS preparing" : {stage("CSS preparing") { sh "cat www/css/* | ${nodejs}/bin/cleancss -o www/min/merged-and-minified.css"}}]
    parallel preparing
    stage('Archiving') {
        sh "mkdir -p artifacts"
        sh "tar --exclude=.git --exclude=www/js --exclude=www/css --exclude=artifacts -czvf artifacts/result.tar.gz ."
        archiveArtifacts artifacts: "artefacts/result.tar.gz", fingerprint: true
    }
    stage('Deploy') {
        sh 'curl -uavattargrey@gmail.com:AP6NqZhP6c5heJFN2okV4fyR3KR -T artifacts/result.tar.gz https://avattar.jfrog.io/artifactory/default-generic-local/result_v_$BUILD_ID.tar.gz'
    }
}
