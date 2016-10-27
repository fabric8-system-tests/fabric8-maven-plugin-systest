def podName = "buildpod.${env.JOB_NAME}.${env.BUILD_NUMBER}".replace('-', '_').replace('/', '_')
echo "using pod name ${podName}"
podTemplate(label: podName, containers: [
    [name: 'maven', image: 'maven', command: 'cat', ttyEnabled: true],
    [name: 'jnlp', image: 'iocanel/jenkins-jnlp-client:latest', command:'/usr/local/bin/start.sh', args: '${computer.jnlpmac} ${computer.name}', ttyEnabled: false]],
    volumes: [
               [$class: 'PvcVolume', mountPath: '/home/jenkins/.mvnrepo', claimName: 'jenkins-mvn-local-repo'],
               [$class: 'SecretVolume', mountPath: '/home/jenkins/.m2/', secretName: 'jenkins-maven-settings']
             ]
  ) {
  node (podName) {
      stage 'clone repo'
      git 'https://github.com/jenkinsci/kubernetes-plugin.git'
        
      container(name: 'maven') {
      stage 'build'
      sh 'mvn -B -U clean install'
    }
  }
}  
