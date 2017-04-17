node {
    
   stage('Git') {
    git 'https://github.com/b4456609/node-js-sample.git'
   }
   stage('Build') {
    sh "sudo docker build -t b4456609/example:latest -t b4456609/example:${BUILD_NUMBER} ."
    TAG = sh (
        script: 'sudo docker inspect --format="{{.Id}}" b4456609/example:latest',
        returnStdout: true
    ).substring(7,14)
    sh "echo ${TAG}"
   }
   stage('upload') {
    sh "sudo docker login -u b4456609 -p b4456609"
    sh "sudo docker push b4456609/example:latest"
    sh "sudo docker push b4456609/example:${BUILD_NUMBER}"
   }
   stage('notifyUrbanCode'){
           
       step([$class: 'UCDeployPublisher',
            siteName: 'test',
            component: [
                $class: 'com.urbancode.jenkins.plugins.ucdeploy.VersionHelper$VersionBlock',
                componentName: 'b4456609/example',
                createComponent: [
                    $class: 'com.urbancode.jenkins.plugins.ucdeploy.ComponentHelper$CreateComponentBlock',
                    componentTemplate: '',
                    componentApplication: 'app'
                ],
                delivery: [
                    $class: 'com.urbancode.jenkins.plugins.ucdeploy.DeliveryHelper$Pull',
                    pullProperties: '',
                    pullSourceType: 'Docker',
                    pullSourceProperties: '',
                    pullIncremental: false
                ]
            ]
        ])
       sleep 200
   }
   stage('deploy'){
        step([$class: 'UCDeployPublisher',
            siteName: 'test',
            deploy: [
                $class: 'com.urbancode.jenkins.plugins.ucdeploy.DeployHelper$DeployBlock',
                deployApp: 'app',
                deployEnv: 'test',
                deployProc: 'deploy',
                createProcess: [
                    $class: 'com.urbancode.jenkins.plugins.ucdeploy.ProcessHelper$CreateProcessBlock',
                    processComponent: 'Deploy'
                ],
                deployVersions: 'b4456609/example:${BUILD_NUMBER}',
                deployOnlyChanged: false
            ]
        ])
   }
}