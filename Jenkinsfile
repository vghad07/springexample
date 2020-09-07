pipeline{
agent any
	tools {

        jdk 'localjdk'
        maven 'localmaven'
	}
	stages{
		stage('Checkout from SCM'){
			steps{
			git branch: 'master', url: 'https://github.com/vghad07/springexample.git'
			}
		}
		stage('build && SonarQube analysis') {
			steps{
			script{
			 def scannerHome = tool 'sonar-scanner';
			withSonarQubeEnv(credentialsId: 'sonar-secret'){
			    sh 'mvn clean package sonar:sonar'
						}
				archiveArtifacts artifacts: '**/*.war', followSymlinks: false
					}
				}
			}
		stage('Quality Gate'){
			steps {
                timeout(time: 1, unit: 'HOURS') {
                    // Parameter indicates whether to set pipeline to UNSTABLE if Quality Gate fails
                    // true = set pipeline to UNSTABLE, false = don't
                    waitForQualityGate abortPipeline: true
                }
			}
		}
	}
}
