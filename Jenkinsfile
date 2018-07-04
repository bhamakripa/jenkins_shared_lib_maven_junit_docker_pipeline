#!groovy

/* import shared library */
@Library('jenkins_shared_lib_mvn_docker')

node() {

	try {
		properties([
		
		buildDiscarder(logRotator(artifactDaysToKeepStr: '30', artifactNumToKeepStr: '10', daysToKeepStr: '30', numToKeepStr: '10')), 
		[$class: 'RebuildSettings', autoRebuild: false, rebuildDisabled: false], 
		
		parameters([	
	
		string(name: 'dockerImage', defaultValue: 'maven:3.5-jdk-7', description: 'Name of the docker image that should be used', trim: true),
		string(name: 'globalSettingsFile', defaultValue: '', description: 'Path or url to the mvn settings file that should be used as global settings file.', trim: true),
		string(name: 'projectSettingsFile', defaultValue: '', description: 'Path or url to the mvn settings file that should be used as project settings file', trim: true),
		string(name: 'pomPath', defaultValue: '', description: 'Path to the pom file that should be used.', trim: true),
		string(name: 'm2Path', defaultValue: '', description: 'Path to the location of the local repository that should be used.', trim: true),
	
		string(name: 'flags', defaultValue: '-o' , description: 'Flags to provide when running mvn.', trim: true),
		string(name: 'goals', defaultValue: 'clean install', description: 'Maven goals that should be executed.', trim: true),
		string(name: 'defines', defaultValue: '', description: 'Additional properties.', trim: true),
		
		booleanParam(name: 'logSuccessfulMavenTransfers', defaultValue: false, description: 'configures maven to log successful downloads. This is set to false by default to reduce the noise in build logs.') 
		])	
		
		])	
		   
	    stage('Checkout Pipeline and shared Library') {
			checkout scm
		}
		
		stage('Execute Maven within Docker') {
            mavenExecute script: this, dockerImage: "${params.dockerImage}", 
            globalSettingsFile: "${params.globalSettingsFile}", projectSettingsFile: "${params.projectSettingsFile}", 
            pomPath: "${params.pomPath}", flags: "${params.flags}",	           
            goals: "${params.goals}", m2Path: "${params.m2Path}", 
            defines: "${params.defines}", logSuccessfulMavenTransfers: "${params.logSuccessfulMavenTransfers}"
		}
	
	}
	catch (error) {	
		currentBuild.result = "FAILED"
		throw error	
	} finally {
		
		/* Notification sent on Slack */   
        slackNotifier(currentBuild.currentResult)
        //cleanWs()
	}	
}

