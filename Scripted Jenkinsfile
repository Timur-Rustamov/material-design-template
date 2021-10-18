node ('slave1'){
    def nodejs = tool 'NodeJS'
    stage('preparing files') {
		def preparing_files=["first":{sh "ls www/js | xargs -I@ ${nodejs}/bin/uglifyjs -o www/min/@ --compress"},
				     "second":{sh "ls www/css | xargs -I@ ${nodejs}/bin/cleancss -o www/min/@"}]
		parallel preparing_files
    }
	stage('archiving the results') {
			sh "tar --exclude=.git --exclude=www/css --exclude=www/js --exclude=Week2_CI_CD_tools/result.tar.gz -czvf Week2_CI_CD_tools/result.tar.gz ."
			archiveArtifacts artifacts: 'Week2_CI_CD_tools/result.tar.gz', fingerprint: true
	}
	}
