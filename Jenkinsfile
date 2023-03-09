node{
   stage('SCM Checkout'){
     git 'https://github.com/bharath-appusamy/my-app.git'
   }
   stage('maven-buildstage'){

      def mvnHome =  tool name: 'maven3', type: 'maven'   
      sh "${mvnHome}/bin/mvn clean package"
	  sh 'mv target/myweb*.war target/newapp.war'
   }
   stage('SonarQube Analysis') {
	        def mvnHome =  tool name: 'maven3', type: 'maven'
	        withSonarQubeEnv('sonar') { 
	          sh "${mvnHome}/bin/mvn sonar:sonar"
	        }
	        
	    }
   stage('Build Docker Image'){
   sh 'docker build -t bharath2867/myweb:0.0.2 .'
   }
   stage('Docker Image Push'){
   withCredentials([string(credentialsId: 'dockerPass', variable: 'dockerPassword')]) {
   sh "docker login -u bharath2867 -p ${dockerPassword}"
    }
   sh 'docker push bharath2867/myweb:0.0.2'
   }
   stage('Nexus Image Push'){
   sh "docker login -u admin -p apache2867 13.57.230.137:8083"
   sh "docker tag bharath2867/myweb:0.0.2 13.57.230.137:8083/damo:1.0.0"
   sh 'docker push 13.57.230.137:8083/damo:1.0.0'
   }
   
   
   stage('Docker deployment'){
   sh 'docker run -d -p 8090:8080 --name anna bharath2867/myweb:0.0.2' 
   }
	   

}
