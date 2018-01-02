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
			
	    }
	}
