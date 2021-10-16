pipeline {
    agent {
        label "Slave"
    }

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
            parallel {
                stage("JS preparing"){
                    steps{
                        sh 'cat www/js/* | uglifyjs -o www/min/merged-and-compressed.js --compress'
                    }
                }
                stage("CSS preparing"){
                    steps{
                        sh 'cat www/css/* | cleancss -o www/min/merged-and-minified.css'
                    }
                }
            }
        }
        stage("Archiving"){
            steps{
                sh "mkdir -p artefacts"
                sh "tar --exclude=.git --exclude=www/js --exclude=www/css --exclude=./artefacts -czvf artefacts/result_v_$BUILD_ID.tar.gz ."                
            }
        }
        stage("Deploy"){
            steps{
                sh 'curl -uavattargrey@gmail.com:AP6NqZhP6c5heJFN2okV4fyR3KR -T artefacts/result_v_$BUILD_ID.tar.gz https://avattar.jfrog.io/artifactory/default-generic-local/result_v_$BUILD_ID.tar.gz'
            }
        }
  }
}
