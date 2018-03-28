node {
   
   stage('Code Checkout') { // for display purposes
      // Get some code from a GitHub repository
    git credentialsId: '2e401477-6a37-4f80-af9e-c7d19b68a711', url: 'https://github.com/snasina/mavenorg.git'

   }
   stage('Build') {
     withMaven(jdk: 'jdk8', maven: 'maven') {
       sh 'mvn clean compile'
     }
   }
   stage('Unit Test') {
     withMaven(jdk: 'jdk8', maven: 'maven') {
       sh 'mvn test'
     }
   }
   stage('SonarQube Analysis') {
      //def job = build job: 'SonarJob'
      //withSonarQubeEnv("SonarQube") {
      //}
      withMaven(jdk: 'jdk8', maven: 'maven') {
          sh ' mvn clean org.jacoco:jacoco-maven-plugin:prepare-agent package org.sonarsource.scanner.maven:sonar-maven-plugin:3.4.0.905:sonar ' +
             ' -Dsonar.host.url=https://sonarcloud.io ' +
             ' -Dsonar.organization=saran2250 '+ 
             ' -Dsonar.login=2131187b401afba5b86667689e3ef316eecad948 '
          }
      }
   
    //stage("Quality Gate"){
          //timeout(time: 1, unit: 'HOURS') {
             // def qg = waitForQualityGate()
              //if (qg.status != 'OK') {
                //  error "Pipeline aborted due to quality gate failure: ${qg.status}"
              //}
         // }
     // }
   
    stage('Archival') {
      withMaven(jdk: 'jdk8', maven: 'maven') {
       //sh 'mvn package'
     }
   }
     stage('Deploy to Artifactory Repo') {
     }

   
    stage('Deploy to Dev') {
      
   }
   stage('Smoke Test Execution') {
      
   }

    
}
