pipeline { 
     agent any
        
        environment { 
        
        registryName = "abhinavsharma9"
      #  registryUrl = 'index.docker.io/v1'
        registryUser = 'abhinavsharma9 '
        registryPassword = 'upAOgMk/okX9LMx1+ACRCQ7B0P'  // these cred are change bcs our git repo is public 
     }
    stages { 
        stage ('checkout') {  
            steps { 
            checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'gitcredential43210', url: 'https://github.com/Abhinavsharma9000/Welcome_Page.git']]])
            } 
        }	
        stage ('Build Docker image') {  
            steps {     
                script {     
                        sh 'docker login -u ${registryUser} -p ${registryPassword}'   
                           sh "docker build -t welcome-ui:${BUILD_TAG} --no-cache ."
                               sh "docker tag welcome-ui:${BUILD_TAG} ${registryName}/demo:latest"
		                 sh 'docker push ${registryName}/demo:latest'
	                  	} 
		             } 
			}  
			
		stage('Deploy to kubernetes Cluster'){ 
		    registryUrl
			steps { 
                             withCredentials([file(credentialsId: 'config', variable: 'config')]) { 
     
	                     sh 'kubectl rollout restart deployment welcome-ui -n welcome --kubeconfig=${config}'
			  } 
	     	       } 
		   } 	
	       } 
	   }
