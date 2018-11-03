node('slave1'){
	stage('Checkout'){
		//Checkout the code from a GitHub repository
		git credentialsId: 'jenkinsGitHub', url: 'https://github.com/anooptcs/WebApp.git'
	}
	stage('Build'){
		def mvnHome = tool name: 'maven-3.5.4', type: 'maven'
        def mvnCMD = "${mvnHome}/bin/mvn"
        sh "${mvnCMD} clean compile"
	}
    stage('Static Analysis'){
		def mvnHome = tool name: 'maven-3.5.4', type: 'maven'
        def mvnCMD = "${mvnHome}/bin/mvn"
        sh "${mvnCMD} clean test checkstyle:checkstyle"
 	}
	stage('Analysis Report'){
        checkstyle canComputeNew: false, defaultEncoding: '', healthy: '', pattern: '', unHealthy: ''
	}
	stage('SonarQB Analysis'){
		def mvnHome = tool name: 'maven-3.5.4', type: 'maven'
        def mvnCMD = "${mvnHome}/bin/mvn"
        sh "${mvnCMD} clean verify sonar:sonar"
	}
    stage('Deploy to Nexus'){
		def mvnHome = tool name: 'maven-3.5.4', type: 'maven'
        def mvnCMD = "${mvnHome}/bin/mvn"
        sh "${mvnCMD} clean deploy"                   
	}
	stage ('Deploye to stageing'){
			
				build job: 'Deploy-to-staging'
			
		}
	
		stage ('Deploye to prod'){
				
					timeout(time:5, unit:'DAYS'){
						input message: 'Approve PROD Deployment?'
					}
				
					build job: 'Deploy-to-Prod'
				
			}
	
}