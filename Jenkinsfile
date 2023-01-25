pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }
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
