pipeline {

    agent any
    
    stages {
    
        stage("is AAGM alive?") {
        
            steps {
				script {
				    echo '------------------------------------------------'
					echo '---------------  is AAGM alive?  ---------------'
					echo '------------------------------------------------'
				
					def response = httpRequest "http://host.docker.internal:8090/api/v1/alive"
					println('Response: '+response.content)
				}
            }
        }
    
        stage("is AAGM ready?") {
        
            steps {
				script {
				    echo '------------------------------------------------'
				    echo '---------------  is AAGM ready?  ---------------'
				    echo '------------------------------------------------'
				
					def response = httpRequest "http://host.docker.internal:8090/api/v1/ready"
					println('Response: '+response.content)
				}
            }
        }
    
        stage("Login and Deploy") {
        
            steps {
				script {
				    echo '------------------------------------------'
				    echo '---------------  Login...  ---------------'
				    echo '------------------------------------------'
				    
					def response = httpRequest "http://host.docker.internal:8090/api/v1/login?user=mirco.hoffmann@apiida.com&pass=masterkeymasterke"
					println('Response Content: '+response.content)

                    echo '-------------------------------------------'
                    echo '---------------  Deploy...  ---------------'
                    echo '-------------------------------------------'

                    def jsonObj = readJSON text: response.content
                    echo 'Token: '+jsonObj.token 
				
				    def body = '{"source": {"type": "git"},"targets": [{targets}],"comment": "Test Migration over API"}'
					
					def migResponse = httpRequest customHeaders:[[name:'X-Token', value:"${jsonObj.token}"]] ,contentType: 'APPLICATION_JSON', httpMode: 'GET', requestBody: "${body}" , url: "http://host.docker.internal:8090/api/v1/apis/{id}/migrate"
					println('Response: '+migResponse.content)
				}
            }
        }
    }
    
}
