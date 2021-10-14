pipeline {
    agent any
    
    tools{
        nodejs "nodejs"
    }

    stages {
        stage("Checkout"){
            steps{
                sh "rm -rf mdt"
                sh "git clone https://github.com/AvaTTaR/mdt || echo already here "
            }
        }
        stage("Preparing"){
            steps{
                parallel (
                    "JS preparing" : {
                        sh 'cat mdt/www/js/* | uglifyjs -o mdt/www/min/merged-and-compressed.js --compress'
                    },
                    "CSS preparing" : {
                        sh 'cat mdt/www/css/* | cleancss -o mdt/www/min/merged-and-minified.css'
                    }
                    )
                
            }
        }
        stage("Archiving"){
            steps{
                sh "tar --exclude=mdt/.git --exclude=mdt/www/js --exclude=mdt/www/css -czvf result.tar.gz mdt/"
            }
        }
        stage("Deploy"){
            steps{
                sh 'curl -uadmin:Password_1 -T result.tar.gz http://192.168.181.132:8082/artifactory/pipe/result_v_$BUILD_ID.tar.gz'
            }
        }
  }
}
