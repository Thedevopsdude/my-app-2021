pipeline{
  agent any

  tools{
    maven 'maven3'
  }
  stages{
    stage('Maven Build'){
      when {
        branch "dev"
      }
      steps{
        sh "mvn clean package"
      }
    }

    stage('Upload To Nexus'){
      when {
        branch "dev"
      }
      steps{
        script{
          def pom = readMavenPom file: 'pom.xml'
          def repository = pom.version.endsWith("SNAPSHOT") ? 'javahome-snapshot' : 'javahome-release'
          nexusArtifactUploader artifacts: 
          [[artifactId: 'myweb', classifier: '', file: 'myweb-0.0.1-SNAPSHOT,war', type: 'war']], 
          credentialsId: 'nexus3', 
          groupId: 'in.javahome', 
          nexusUrl: '172.31.29.72:8081', 
          nexusVersion: 'nexus3', protocol: 'http', 
          repository: javahome-snapshot, 
          version: 0.0.1-SNAPSHOT
        }
      }
    }
    
    stage('dev-deploy'){
      when {
        branch "dev"
      }
      steps{
        echo "deploy to dev environment"
      }
    }

    stage('uat-deploy'){
      when {
        branch "uat"
      }
      steps{
        echo "deploy to uat environment"
      }
    }

    stage('prd-deploy'){
      when {
        branch "master"
      }
      steps{
        echo "deploy to prod environment"
      }
    }
  }
}
