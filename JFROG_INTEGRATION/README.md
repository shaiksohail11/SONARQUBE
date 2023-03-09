INTEGRATING JFROG-CLOUD WITH JENKINS
------------------------------------

## Pre-requisite:
-----------------

* Two linux servers
    * One with Java_11,Maven and Jenkins installed i.e (Jenkins_Master)
    * One with Maven,Java-11 or greater than 11 i.e (Jenkins_node)
    * [Refer Here](https://www.jenkins.io/doc/book/installing/linux/) For jenkins installation on linux
    * Configure `jenkins-node on jenkins-master`
![preview](Images.png/jfrog1.png)

NOW CREATE JFROG ACCOUNT
------------------------

[Refer Here](https://jfrog.com/artifactory/?utm_source=google&utm_medium=cpc&utm_campaign=Search|DSK|BrandAwareness|EM|India|202208&utm_term=jfrog&utm_network=g&cq_plac=&cq_plt=gp&utm_content=u-bin&gclid=Cj0KCQiApKagBhC1ARIsAFc7Mc4FjkHHWJLE2nO2WEd2FeZhZPsk3FTMKPMRNqRGxf1NFzWe6jPvA1MaAo0KEALw_wcB) For Docs to creating Jfrog-Account.

* `Note` : Jfrog gives you 14 days free trail.
  
## steps to create Jfrog account:

![preview](Images.png/jfrog2.png)
![preview](Images.png/jfrog3.png)
![preview](Images.png/jfrog4.png)
![preview](Images.png/jfrog5.png)

## Now let us setup maven repository
![preview](Images.png/jfrog6.png)
![preview](Images.png/jfrog7.png)
![preview](Images.png/jfrog8.png)
![preview](Images.png/jfrog10.png)

* Download the snippet and view with any editor and make a note of `Username and Password` we will be using it further.
![preview](Images.png/jfrog11.png)
![preview](Images.png/jfrog12.png)

## NOW LET US INTEGRATE JFOG WITH JENKINS
-----------------------------------------
* Pre-requisite:
   * Artifact plugin
   * maven plugin
  
![preview](Images.png/jfrog13.png)
![preview](Images.png/jfrog14.png)

* You can check it in the job section

![preview](Images.png/jfrog15.png)

* Now let us configure credentials and Configure-system.
  
![preview](Images.png/jfrog16.png)
![preview](Images.png/jfrog17.png)
![preview](Images.png/jfrog24.png)

* Configure maven path in `Global Tool Configuration`
* You can view the path by typing `whereis mvn` in the node 
  
![preview](Images.png/jfrog19.png)
![preview](Images.png/jfrog20.png)

* Now create a new job with maven project

![preview](Images.png/jfrog21.png)
![preview](Images.png/jfrog22.png)
![preview](Images.png/jfrog23.png)
![preview](Images.png/jfrog25.png)
![preview](Images.png/jfrog26.png)

* Now click on `post build action` and select `Deploy artifacts to artifactory`
  
![preview](Images.png/jfrog28.png)
![preview](Images.png/jfrog29.png)
![preview](Images.png/jfrog30.png)
![preview](Images.png/jfrog31.png)

* Now we can check the artifact in the jenkins and also in Jfrog-Arifactory.
  
![preview](Images.png/jfrog32.png)
![preview](Images.png/jfrog33.png)

* Navigate to Jfrog artifactory and under build section we can find our artifacts.
  
  * For declarative pipeline refer below

```
pipeline {
    agent { label 'NODE1' }
    triggers { pollSCM ('* * * * *') }
    parameters {
        choice(name: 'MAVEN_GOAL', choices: ['package', 'install', 'clean'], description: 'Maven Goal')
    }
    stages {
        stage('vcs') {
            steps {
                git url: 'https://github.com/shaiksohail11/spring-petclinic.git',
                    branch: 'main'
            }
        }
        stage ('Artifactory configuration') {
            steps {
                rtServer (
                    id: "ARTIFACTORY_SERVER",
                    url: 'https://storingartifacts.jfrog.io/artifactory',
                    credentialsId: 'JFROG-CREDENTIALS'
                )

                rtMavenDeployer (
                    id: "MAVEN_DEPLOYER",
                    serverId: "ARTIFACTORY_SERVER",
                    releaseRepo: 'libs-release',
                    snapshotRepo: 'libs-snapshot'
                )

                rtMavenResolver (
                    id: "MAVEN_RESOLVER",
                    serverId: "ARTIFACTORY_SERVER",
                    releaseRepo: 'libs-release',
                    snapshotRepo: 'libs-snapshot'
                )
            }
        }
        stage('package') {
            tools {
                jdk 'NODE!'
            }
            steps {
                rtMavenRun (
                    tool: 'MAVEN_DEFAULT',
                    pom: 'pom.xml',
                    goals: 'clean install',
                    deployerId: "MAVEN_DEPLOYER"
                    
                )
                rtPublishBuildInfo (
                    serverId: "ARTIFACTORY_SERVER"
                )
                //sh "mvn ${params.MAVEN_GOAL}"
            }
        }
        stage('sonar analysis') {
            steps {
                // performing sonarqube analysis with "withSonarQubeENV(<Name of Server configured in Jenkins>)"
                withSonarQubeEnv('SONAR_CLOUD') {
                    sh 'mvn clean package sonar:sonar -Dsonar.organization=springpetclinic1'
                }
            }
        }
        stage('post build') {
            steps {
                archiveArtifacts artifacts: '**/target/spring-petclinic-3.0.0-SNAPSHOT.jar',
                                 onlyIfSuccessful: true
                junit testResults: '**/surefire-reports/TEST-*.xml'
            }
        }
    }
}

```


---------------------------------------------------------THE END----------------------------------------------------------------------------------



