/* import shared library */
@Library('jenkins_shared_lib_mvn_docker')_

pipeline {
	
	agent { label 'master' }
    
	options {
	   buildDiscarder(logRotator(artifactDaysToKeepStr: '30', artifactNumToKeepStr: '10', daysToKeepStr: '30', numToKeepStr: '10'))
	}	
	
	parameters { 
	
	    string(name: 'dockerImage', defaultValue: 'maven:3.5-jdk-7', description: 'Name of the docker image that should be used')
	    string(name: 'globalSettingsFile', defaultValue: '', description: 'Path or url to the mvn settings file that should be used as global settings file.')
	    string(name: 'projectSettingsFile', defaultValue: '', description: 'Path or url to the mvn settings file that should be used as project settings file')
	    string(name: 'pomPath', defaultValue: '', description: 'Path to the pom file that should be used.')
	    string(name: 'm2Path', defaultValue: '', description: 'Path to the location of the local repository that should be used.')

	    string(name: 'flags', defaultValue: '-o' , description: 'Flags to provide when running mvn.')
	    string(name: 'goals', defaultValue: 'clean install', description: 'Maven goals that should be executed.')
	    string(name: 'defines', defaultValue: '', description: 'Additional properties.')
    	
    	booleanParam(name: 'logSuccessfulMavenTransfers', defaultValue: false, description: 'configures maven to log successful downloads. This is set to false by default to reduce the noise in build logs.') 
	    
	}	    
	   
	stages {
	    stage('Execute Maven within Docker') {
	        steps {
	            mavenExecute script: this, dockerImage: "${params.dockerImage}", 
	            globalSettingsFile: "${params.globalSettingsFile}", projectSettingsFile: "${params.projectSettingsFile}", 
	            pomPath: "${params.pomPath}", flags: "${params.flags}",	           
	            goals: "${params.goals}", m2Path: "${params.m2Path}", 
	            defines: "${params.defines}", logSuccessfulMavenTransfers: "${params.logSuccessfulMavenTransfers}"
	        }
		}
	}
	post {
	    always {
	   		/* Notification sent on Slack */   
	        slackNotifier(currentBuild.currentResult)
	        //cleanWs()
	    }
	}
}

