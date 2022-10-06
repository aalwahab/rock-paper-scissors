pipeline {
  agent any
  tools { 
    maven 'myMaven' 
  }
  parameters {
    string(name: "ARTIFACT_NAME", defaultValue: "roshambo-0.0.0.war")
    string(name: "PIPELINE_RUNTIME", defaultValue: "")
    string(name: "VERSION", defaultValue: "0.0.0")
    choice(name: "ENV", choices: ["dev", "qa", "uat", "prod"])
    string(name: "GIT_SHA", defaultValue: "1234")
  }
  stages {
    stage ('Clean Workspace') {
      steps {
        cleanWs()
      }
    }
    stage ('Git Checkout') {
      steps {
        git changelog: false, credentialsId: "b3069e26-961a-489b-8ce7-1255a6d581ad", poll: false, url: "https://github.com/aalwahab/rock-paper-scissors.git"
      }
    }
    stage ('Build') {
      steps {
        sh """
        mvn versions:set -DnewVersion=${params.VERSION}-${params.ENV}-${params.GIT_SHA}
        mvn clean install """
      }
    }
    
   stage ('Deploy to Artifactory') {
      steps {
        echo "Hello WOrld"
        rtUpload (
          serverId: "myCloudInstance",
          spec: '''{
            "files": [
              {
                "pattern": "target/*.war",
                "target": "example-repo-local/roshambo/"
              }
            ]
    
          }'''
        )
      }
    }
  }
}


