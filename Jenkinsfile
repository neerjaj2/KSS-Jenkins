pipeline {

agent any

    stages {

        stage ('Workspace Cleanup') {
          steps {
              deleteDir()
    	         }
                                    }

    	stage ('SCM Checkout') {
		steps {
			script {
			try {
				timeout(time: 20, unit: 'SECONDS') {

			checkout([$class: 'GitSCM', branches: [[name: '*/prakashul-qa']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'e33a422c-b11f-4a16-8637-4037f9ddaf66', url: 'https://github.com/nitinbhadauria/KSS-Jenkins.git']]])
  	   		}

			}

			catch(err) {
                err.printStackTrace()
                                                }
                sh 'echo Proceeding'
               }
            }
                          }

	stage("build_artifact") {
    agent { docker "maven:3-jdk-8" }
            steps {
                sh 'mvn package'
                sh 'ls -R *'

                  }
                }

    stage("build_push_image") {

      agent any

        steps {
              withDockerRegistry([credentialsId: 'b6ef8f34-268d-4a12-a02f-c0eb8bf002ec', url: "https://hub.docker.com/"]) {

                script {
        // we give the image the same version as the .war package
              def image = docker.build("prakashul/knowledgemeet:latest",'.')
              image.push()
}
  //                script {
    //             sh '/usr/bin/docker build --no-cache -t prakashul/knowledgemeet .'
    //            withDockerRegistry([credentialsId: 'b6ef8f34-268d-4a12-a02f-c0eb8bf002ec', url: "https://hub.docker.com/"]) {
  // we give the image the same version as the .war package


    //            }
            //  }
            }
}
}

    stage("deploy") {
      agent any
      steps {
        sh '. /var/lib/jenkins/deploy.sh'
      }

    }

            }



    post {
        always {
            archive "target/**/*.jar"
            // junit 'target/surefire-reports/*.xml'
        }
    }
}
