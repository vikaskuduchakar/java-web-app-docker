node{
    
    def Maven_Home= tool name: 'maven-3.6.1',type: 'maven' 
    stage('gitcheckout'){
        
       git credentialsId: '5e7cfcb2-a74a-485d-9e74-168e3683b0ea', url: 'https://github.com/mallanagouda4/java-web-app-docker.git' 
    }
    
    stage ('Build'){
        
        sh "${Maven_Home}/bin/mvn clean package"
    }
    
    stage ('imgbuild'){
       
       sh "docker build -t 13.127.217.37:8083/gichgiligili:$BUILD_NUMBER ." 
        
    }
    
    stage ('dockerpush'){
        withCredentials([string(credentialsId: 'nexusid', variable: 'pwd')]) {
                      
            sh "docker login -u admin -p ${pwd} 13.127.217.37:8083 "
                
        sh " docker push 13.127.217.37:8083/gichgiligili:$BUILD_NUMBER"       
   
        }
    }

    stage ('pullimg'){
 withCredentials([string(credentialsId: 'nexusid', variable: 'pwd')]) {
          
            
           // sh "docker login -u admin -p ${pwd} 13.127.217.37:8083 "
    
            
        sh " docker pull 13.127.217.37:8083/gichgiligili:$BUILD_NUMBER"       
    }

}
     stage('deployment'){
         def dockerRun = 'docker run -d -p 8080:8080 --name maven-web-docerimage 13.127.217.37:8083/gichgiligili:$BUILD_NUMBER'
        sshagent(['pemkey']) {
          sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.36.223 docker stop maven-web-docerimage || true'
          sh 'ssh  ubuntu@172.31.36.223 docker rm maven-web-docerimage || true'
          sh 'ssh  ubuntu@172.31.36.223 docker rmi -f  $(docker images -q) || true'
          sh "ssh  ubuntu@172.31.36.223 ${dockerRun}"
           
			}
	}
}
