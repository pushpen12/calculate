pipeline {
    agent any
	
    stages {
		stage('Unit Tests') {
            steps{
                sh 'mvn -f pom.xml clean test'
            }
        }
  //      
        //stage("sonar_static_check"){
          //  steps{
	//	withSonarQubeEnv('MySonarQube') {
                   // Optionally use a Maven environment you've configured already
       //           sh 'mvn -f maths/pom.xml clean sonar:sonar -Dmaven.test.skip=true'
     //        }
   //     }
 //       }
	
//	stage("Quality Gate") {
  //          steps {
    //            timeout(time: 2, unit: 'MINUTES') {
      //              // Parameter indicates whether to set pipeline to UNSTABLE if Quality Gate fails
        //            // true = set pipeline to UNSTABLE, false = don't
          //          waitForQualityGate abortPipeline: true
            //    }
            //}
        //}

		
        stage ('Artifactory configuration') {
            steps {
                rtServer (
                    id: "ARTIFACTORY_SERVER",
                    url: "http://192.168.0.101/artifactory",
		    credentialsId: 'artifactory-id'
                )

                rtMavenDeployer (
                    id: "MAVEN_DEPLOYER",
                    serverId: "ARTIFACTORY_SERVER",
                    releaseRepo: "libs-release-local",
                    snapshotRepo: "libs-snapshot-local"
                )

                rtMavenResolver (
                    id: "MAVEN_RESOLVER",
                    serverId: "ARTIFACTORY_SERVER",
                    releaseRepo: "libs-release",
                    snapshotRepo: "libs-snapshot"
                )
            }
        }

        stage ('Build & Upload Artifact') {
            steps {
                rtMavenRun (
                    tool: "maven3.6", // Tool name from Jenkins configuration
                    pom: 'maths/pom.xml',
                    goals: 'clean install -Dmaven.test.skip=true',
                    deployerId: "MAVEN_DEPLOYER",
                    resolverId: "MAVEN_RESOLVER"
                )
            }
        }

        stage ('Publish build info') {
            steps {
                rtPublishBuildInfo (
                    serverId: "ARTIFACTORY_SERVER"
                )
            }
        }
    }
}
