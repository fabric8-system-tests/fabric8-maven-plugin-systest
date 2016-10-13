def podName = "buildpod.${env.JOB_NAME}.${env.BUILD_NUMBER}".replace('-', '_').replace('/', '_')
echo "using pod name ${podName}"
podTemplate(label: podName, containers: [
    [name: 'maven', image: 'maven', ttyEnabled: true, command: 'cat'], 
    [name: 'jnlp', image: 'iocanel/jenkins-jnlp-client:latest', command:'/usr/local/bin/start.sh', args: '${computer.jnlpmac} ${computer.name}', ttyEnabled: false]
  ]) {
  node (podName) {
      stage 'clone repo'
      git 'https://github.com/jenkinsci/kubernetes-plugin.git'
        
      container(name: 'maven') {
      stage 'build'
      sh 'mvn -BU clean install'
    }
  }
}  
