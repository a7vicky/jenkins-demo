pipeline {
    agent any
    stages {
        stage ('S2I build sample java app') {
            steps {
                script {
                    openshift.withCluster( 'vickyocp' ) {
                            openshift.verbose()
                            openshift.withCredentials( 'XeM0vGt7q_rFmFTIYiMX8I1eBPaqhC5WFcPqZg8MKTQ' ) {
                            openshift.logLevel(2)
                            openshift.newProject ('myproject')
                            openshift.withProject( 'myproject' ) {
                                def created = openshift.newApp( 'jboss-webserver30-tomcat8-openshift:1.2~https://github.com/a7vicky/jenkins-demo#master' )
                                echo "new-app created ${created.count()} objects named: ${created.names()}"
                                created.describe()
                                def bc = created.narrow('bc')
                                def result = bc.logs('-f')
                                echo "The logs operation require ${result.actions.size()} oc interactions"
                                echo "Logs executed: ${result.actions[0].cmd}"
                                def logsString = result.actions[0].out
                                def logsErr = result.actions[0].err
                                def buildSelector = bc.startBuild()
                                buildSelector.logs('-f')
                            }
                        } 
                    }
                }
            }
        }
    }
}
