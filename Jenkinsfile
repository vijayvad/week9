podTemplate(yaml: '''
    apiVersion: v1
    kind: Pod
    spec:
      containers:
      - name: gradle
        image: gradle:jdk8
        command:
        - sleep
        args:
        - 99d
      restartPolicy: Never
    ''') {
  node(POD_LABEL) {

  stage('Start a gradle project') {
      git branch: 'main', url: 'https://github.com/vijayvad/week9.git'
      container('gradle') {
        stage('Start Calculator') {
		    sh '''
        echo 'Start Calculator'
				curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
        chmod +x ./kubectl
	chmod +x gradlew
        ./kubectl apply -f calculator.yaml -n staging
        ./kubectl apply -f hazelcast.yaml -n staging
	        '''
       } 
 stage("Test using Curl Command") {
		sh '''
                echo 'Test using Curl'
				test $(curl calculator-service.staging.svc.cluster.local:8080/sum?a=6\\&b=2) -eq 8 && echo 'pass' || 'fail'
				test $(curl calculator-service.staging.svc.cluster.local:8080/div?a=6\\&b=6) -eq 1 && echo 'pass' || 'fail'
         '''
            }
     }
    }        
  }
} 
