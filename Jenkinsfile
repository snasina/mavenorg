node {
    def server = Artifactory.server('saran.jfrog.io')
    def buildInfo = Artifactory.newBuildInfo()
    def rtMaven = Artifactory.newMavenBuild()
    
    stage ('Checkout & Build') {
        git url: 'https://github.com/snasina/mavenorg.git'
    }
 
    stage ('Unit Test') {
        rtMaven.tool = 'maven' // Tool name from Jenkins configuration
        rtMaven.run pom: 'pom.xml', goals: 'clean test'
    }
    
    stage('SonarQube Analysis') {
        rtMaven.run pom: 'pom.xml', goals: 'clean org.jacoco:jacoco-maven-plugin:prepare-agent package sonar:sonar -Dsonar.organization=saran2250-github -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=2131187b401afba5b86667689e3ef316eecad948 '
     
          
      }
    
    stage ('Artifactory configuration') {
        // Obtain an Artifactory server instance, defined in Jenkins --> Manage..:
         
        rtMaven.tool = 'maven' // Tool name from Jenkins configuration
        rtMaven.deployer releaseRepo: 'libs-release-local', snapshotRepo: 'libs-snapshot-local', server: server
        rtMaven.resolver releaseRepo: 'libs-release', snapshotRepo: 'libs-snapshot', server: server
        rtMaven.deployer.deployArtifacts = false // Disable artifacts deployment during Maven run
     }
            
    stage ('Install') {
        rtMaven.run pom: 'pom.xml', goals: 'install', buildInfo: buildInfo
     }
 
    stage ('Deploy') {
        rtMaven.deployer.deployArtifacts buildInfo
    }
        
    stage ('Publish build info') {
        server.publishBuildInfo buildInfo
    }
}
