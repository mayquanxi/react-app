pipeline {
	agent {
		docker {
			image 'node:13'
			label 'akali'
			args '-p 3000:3000'
		}
	}
	stages {
		stage('BUILD') {
			steps {
				echo "Install dependences and moduler for project"
				sh 'npm install'
			}
		}
		stage('TEST') {
			steps {
				echo "Test project in development"
				sh 'npm start & sleep 10'
				sh 'echo &! > .pidfile'
				input message: 'Continue or Abort'
				sh 'kill -9 $(cat .pidfile)'
			}
		}
		stage('DEPLOY') {
			steps {
				echo "Build apps for production "
				sh 'npm run build'
				echo "Test production BUILD in build dir"
				sh 'cd build'
				sh 'npm start & sleep 10'
				sh 'echo &! > .pidfile'
				input message: 'Continue or Abort'
				sh 'kill -9 $(cat .pidfile)'
				echo 'production is success for deploy another server in build folder'
			}
		}
	}
}