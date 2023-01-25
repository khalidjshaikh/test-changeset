podTemplate(
  containers: [
                containerTemplate( name: 'container-name', 
                                   image: '384339018592.dkr.ecr.us-east-2.amazonaws.com/gradle', 
                                   ttyEnabled: true, 
                                   command: 'cat',
                                   alwaysPullImage: true)
              ],
  volumes: [
    hostPathVolume(hostPath: '/var/run/docker.sock', mountPath: '/var/run/docker.sock'),
    // hostPathVolume(hostPath: '/home/gradle/.gradle', mountPath: '/home/gradle/.gradle')
  ],
  yaml: '''
spec:
  affinity:
    nodeAffinity:
      preferredDuringSchedulingIgnoredDuringExecution:
      - weight: 1
        preference:
          matchExpressions:
          - key: kubernetes.io/nodepool
            operator: In
            values:
            - jenkins
  '''
  ) {
  node(POD_LABEL) {
    container('conatiner-name') {
        sh 'ls'
      }
    }
  }
}