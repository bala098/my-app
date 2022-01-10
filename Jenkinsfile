node{
    stage('SCM Checkout'){
     git 'https://github.com/damodaranj/my-app.git'
   }
   stage('Compile-Package'){

      def mvnHome =  tool name: 'maven3', type: 'maven'   
      sh "${mvnHome}/bin/mvn clean package"
	  sh 'mv target/myweb*.war target/new.war'
   }
   stage('SonarQube Analysis') {
	        def mvnHome =  tool name: 'maven3', type: 'maven'
	        withSonarQubeEnv('sonar') { 
	          sh "${mvnHome}/bin/mvn sonar:sonar"
	        }
	    }
   stage('Build Docker Imager'){
   sh 'docker build -t bala/myweb:0.0.2 .'
   }
   stage('Docker Image Push'){
   withCredentials([string(credentialsId: 'dockerPass', variable: 'dockerPassword')]) {
   sh "docker login -u balamurugan97 -p '${dockerPassword}'"
    }
   sh 'docker tag bala/myweb:0.0.2 balamurugan97/bala'
   sh 'docker push balamurugan97/bala'
   }
    stage('Nexus Image Push'){
   sh "docker login -u admin -p '#sbm1997' 13.214.164.50:8083"
   sh "docker tag balamurugan97/bala 13.214.164.50:8083/bala"
   sh 'docker push 13.214.164.50:8083/bala'
   }
   stage('Remove Previous Container'){
	try{
		sh 'docker rm -f tomcattest'
	}catch(error){
		//  do nothing if there is an exception
	}
   stage('Docker deployment'){
   sh 'docker run -d -p 8090:8080 --name tomcattest balamurugan97/bala' 
   }
}
}
