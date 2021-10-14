pipeline {
    agent any
    triggers {
        pollSCM('* * * * *')
        githubPush()
    }
    tools{
        nodejs "nodejs"
    }

    stages {
        stage("Checkout"){
            steps{
                checkout scm
                sh 'echo test triggers'
            }
        }
        stage("Preparing"){
            steps{
                parallel (
                    "JS preparing" : {
                        sh 'cat www/js/* | uglifyjs -o www/min/merged-and-compressed.js --compress'
                    },
                    "CSS preparing" : {
                        sh 'cat www/css/* | cleancss -o www/min/merged-and-minified.css'
                    }
                    )
                
            }
        }
        stage("Archiving"){
            steps{
                sh "tar --exclude=.git --exclude=www/js --exclude=www/css -czvf /tmp/result.tar.gz ."
            }
        }
        stage("Deploy"){
            steps{
                sh 'curl -uadmin:Password_1 -T /tmp/result.tar.gz http://165.232.70.142:8082/artifactory/pipe/result_v_$BUILD_ID.tar.gz'
            }
        }
  }
}
