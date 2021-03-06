pipeline {
    agent any
    stages {
	stage('Pull code'){
		steps{
			echo 'downloading git directory..'
			git 'https://github.com/ravisinghrajput95/python-sast.git'
		}
	}
	stage('Check dependency'){
		steps{
			echo 'running python safety check on requirements.txt file'
			sh 'safety check -r requirements.txt'
		}
	}
	stage('Install dependency'){
		steps{
			sh """
        		virtualenv --no-site-packages .
        		source bin/activate
        		pip install -r requirements.txt
        		deactivate
        		"""
		}
	}
        stage('Run python project') {
            steps {
                echo 'Running python script'
		sh """
		source bin/activate
		python hello.py
		deactivate
		"""
            }
        }
        stage('Test insecure-package') {
            steps {
                echo 'Testing insecure dependency'
		sh """
		source bin/activate
		pip install insecure-package
		safety check
		"""
            }
        }
    }
    post {
        always {
            echo 'This will always run'
        }
        success {
            echo 'This will run only if successful'
        }
        failure {
            echo 'This will run only if failed'
        }
        unstable {
            echo 'This will run only if the run was marked as unstable'
        }
        changed {
            echo 'This will run only if the state of the Pipeline has changed'
            echo 'For example, if the Pipeline was previously failing but is now successful'
        }
    }
}
