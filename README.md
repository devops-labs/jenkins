# jenkins
Jenkins research work
Table of Contents
Job DSL : Example code 
-----------------------
def project = 'jenkins'
def branchApi = new URL("https://api.github.com/repos/devops-labs/${project}/branches")
def branches = new groovy.json.JsonSlurper().parse(branchApi.newReader())

branches.each {
    def branchName = it.name
    String jobName = "${project}-${branchName}".replaceAll('/', '-')
    
  
// folder creation in Jenkins
    folder(project) {
    description 'This example shows basic folder/job creation.'
	}
  
// Job creation in Jenkins and steps to execute in each job.
    job("$project/$jobName") {
        scm {
            git {
                branch(branchName)
                remote {
                    url("git://github.com/devops-labs/${project}.git")
                    credentials("XYZ")
                }
            }
          steps {
            gradle("clean -Dproject.name=${project}/${branchName}")
        }
        }
// End of Job Creation 
    }
}
------------------------------
