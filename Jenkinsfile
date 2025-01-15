pipeline {
    stages {
        stage('Build') {
			agent {
        		docker {
           	    image 'node:18-alpine'
                reuseNode true
            }
   	    }
            steps {
                sh '''
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }
        stage('Test') {
			agent {
        		docker {
           	    image 'node:18-alpine'
                reuseNode true
            }
		}
            steps {
                sh '''
                    test -f build/index.html
                    npm test
                '''
            }
        }
		 stage('E2E') {
            steps {
				agent {
					docker {
						image 'mcr.microsoft.com/playwright:v1.49.1-noble'
						reuseNode true
					}
				}
                sh '''
                   npm install -g serve
				   serve -s build
				   npx playwright test
                '''
            }
        }
    }
	
	post {
		always {
			junit 'test-results/junit.xml'
		}
	}
}
