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
        archiveArtifacts artifacts: "artifacts/result.tar.gz", fingerprint: true
    }
    stage('Deploy') {
        def artserver = Artifactory.server 'jfrog'
        def uploadSpec = """{
                "files": [
                    {
                    "pattern": "artifacts/result.tar.gz",
                    "target": "default-generic-local/result_v_${BUILD_ID}.tar.gz"
                    }
                ]
            }"""
        artserver.upload spec: uploadSpec, failNoOp: true
    }
}
