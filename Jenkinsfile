pipeline {
    agent any
    
    tools{
        nodejs "nodejs"
    }

    stages {
        stage("Checkout"){
            steps{
                checkout scm
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
                sh 'curl -uadmin:Password_1 -T /tmp/result.tar.gz http://192.168.181.132:8082/artifactory/pipe/result_v_$BUILD_ID.tar.gz'
            }
        }
  }
}
