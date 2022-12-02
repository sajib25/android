pipeline {
  agent any
  environment {
    APP_NAME = 'simple_zip'
  }
  tools{
  jdk 'java'}
  
  options {
    // Stop the build early in case of compile or test failures
    skipStagesAfterUnstable()
  }
  stages {
   
    // Put your stages here
    
    stage('Detect build type') {
	  steps {
	    script {
	      if (env.BRANCH_NAME == 'develop' || env.CHANGE_TARGET == 'develop') {
		env.BUILD_TYPE = 'debug'
	      } else if (env.BRANCH_NAME == 'main' || env.CHANGE_TARGET == 'main') {
		env.BUILD_TYPE = 'release'
	      }
	    }
	  }
    }
    
      stage('Permission to execute gradlew') {
            steps {
                sh 'chmod 777 ./gradlew'
            }
        }
    
	    stage('Compile') {
	  steps {
	    // Compile the app and its dependencies
	    sh './gradlew compile${BUILD_TYPE}Sources'
	  }
	}
	
		stage('Build') {
		  steps {
		    // Compile the app and its dependencies
		    sh './gradlew assemble${BUILD_TYPE}'
		    sh './gradlew generatePomFileForLibraryPublication'
		  }
		}
		
		stage('Publish') {
  steps {
    // Archive the APKs so that they can be downloaded from Jenkins
    archiveArtifacts "**/${APP_NAME}_${BUILD_TYPE}.apk"
    // Archive the ARR and POM so that they can be downloaded from Jenkins
    // archiveArtifacts "**/${APP_NAME}_${BUILD_TYPE}.aar, **/*pom-   default.xml*"
  }
}
  }
}
