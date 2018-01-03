	pipeline {
	    agent any
	    tools {
	        maven 'Maven3'
	        jdk 'jdk1.8.0_101'
	    }
	    stages {
	        
	        stage ('Build') {
	            steps {
					bat 'mvn install'
	            }
	           
	        }
	        stage ('unit test') {
	            steps {
					bat 'mvn compile'
	            }
	            post {
	                success {
	                    junit 'target/surefire-reports/*.xml' 
	                }
	            }
	        }

			stage('cobertura'){
				steps{
					bat 'mvn cobertura:cobertura -Dcobertura.report.format=xml'
				}
				post{
					success {
						step([$class: 'CoberturaPublisher', autoUpdateHealth: false, autoUpdateStability: false, coberturaReportFile: 'target/site/cobertura/*.xml', failUnhealthy: false, failUnstable: false, maxNumberOfBuilds: 0, onlyStable: false, sourceEncoding: 'ASCII', zoomCoverageChart: false])
					}
	}
			}
			stage ('generate documentation') {
				steps {
					bat 'mvn javadoc:javadoc'
				}
				post{
					success{
						step([$class: 'JavadocArchiver', javadocDir: 'target/site/apidocs', keepAll: false])
					}
				}
			}
			stage('package'){
				steps{
					bat 'mvn package'
				}

			}
			
			stage('deploy'){
				 steps{
			   		 nexusPublisher nexusInstanceId: 'localnexus',
				    nexusRepositoryId: 'releases',
				    packages: [[$class: 'MavenPackage',
						mavenAssetList: [[classifier: '',
								  extension: 'jar',
								  filePath: 'target/15ShadesQcm-0.0.1-SNAPSHOT.jar']],
						mavenCoordinate: [artifactId: '15ShadesQcm',
								  groupId: 'com.fboot',
								  packaging: 'jar',
								  version: '2.2']
					       ]]
}



	    }
			}
	}
