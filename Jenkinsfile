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
  container('container-name') {
      sh 'printenv|sort'
      sh 'ls'
      sh 'pwd'

           stages {
         stage("Test changeset") {
             when {
                 changeset "**/Jenkinsfile"
             }
             steps {
                 echo("changeset works")
             }
         }

         stage("Display changeset?") {
             steps {
                 script {
                     def changeLogSets = currentBuild.changeSets
                     echo("changeSets=" + changeLogSets)
                     for (int i = 0; i < changeLogSets.size(); i++) {
                         def entries = changeLogSets[i].items
                         for (int j = 0; j < entries.length; j++) {
                             def entry = entries[j]
                             echo "${entry.commitId} by ${entry.author} on ${new Date(entry.timestamp)}: ${entry.msg}"
                             def files = new ArrayList(entry.affectedFiles)
                             for (int k = 0; k < files.size(); k++) {
                                 def file = files[k]
                                 echo " ${file.editType.name} ${file.path}"
                             }
                         }
                     }
                 }
             }
         }
      }
    }
  }
}
