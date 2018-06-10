node {
	stage('Preparation') {
	    sh "pwd"
		// Created folders needed by containers
		sh "mkdir -p ~/e2etest_vol/tests/"
		sh "mkdir -p ~/e2etest_vol/reports/"
		// Create reports folder
		sh "mkdir -p reports/${BUILD_NUMBER}"

		// Get some code from a GitHub repository
		// - test scripts
		git credentialsId: '7a693330-0a21-463f-9e2d-0a724a83c5f9', url: 'https://github.com/anashamdaoui/robotframework_tests.git'
		sh "cp *.robot ~/e2etest_vol/tests/"
	
		// - containers to load
		git credentialsId: '7a693330-0a21-463f-9e2d-0a724a83c5f9', url: 'https://github.com/anashamdaoui/ui_tests_framework.git'
	    sh "docker-compose -f docker-compose.yml ps"
	    sh "docker-compose -f docker-compose.yml down"
	    
	}
   	stage('Docker Compose') {
      	// Run docker containers
		sh "docker-compose -f docker-compose.yml up -d"
		// Check containers health and display some info
		sh "docker-compose ps"
		sh "docker inspect selenium-hub"
   	}
   	stage('Run Tests') {
	    	sh "docker exec -u root -it selenium-hub pybot -d /root/reports/ /root/tests/0_testhub.robot"
    		sh "cp ~/e2etest_vol/reports/ reports/${BUILD_NUMBER}"
   	}
   	stage('Results') {
		step(
			[
			$class : 'RobotPublisher',
			outputPath : reports/${BUILD_NUMBER},
			outputFileName : "*.xml",
			disableArchiveOutput : false,
			passThreshold : 100,
			unstableThreshold: 95.0,
			onlyCritical : true,
			otherFiles : "*.html",
			]
		)
   	}
	stage('Clean') {
		sh "rm -rf ~/e2etest_vol"
		sh "docker-compose -f docker-compose.yml down"
   	}
}

