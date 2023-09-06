# SONARQUBE-SONAR_CLOUD
INTEGRATE SONAR_CLOUD WITH JENKINS AND PUBLISH TEST RESULTS TO SONAR_CLOUD
----------------------------------------------------------------------------

* To integrate Sonar_cloud with jenkins we need:
* Pre-requisite
  --------------
* Two linux servers, 
    * One with Java_11,Maven and Jenkins installed i.e (Jenkins_Master)
    * one with Maven,Java-11 or greater than 11 i.e (node)
    * Configure `jenkins-node on jenkins-master`
  


## NOW CREATE SONAR_CLOUD ACCOUNT
----------------------------------
[Refer Here](https://www.sonarsource.com/products/sonarcloud/) for official docs to create sonar-cloud account 

![preview](images.png/sonar2.png)
![preview](images.png/sonar3.png)
![preview](images.png/sonar4.png)
![preview](images.png/sonar5.png)
![preview](images.png/sonar6.png)
![preview](images.png/sonar7.png)

* Here Iam using GIT-HUB you can choose anything from the options
* Provide your git-hub credentials
![preview](images.png/sonar8.png)
![preview](images.png/sonar9.png)
![preview](images.png/sonar10.png)
![preview](images.png/sonar11.png)
![preview](images.png/sonar12.png)
![preview](images.png/sonar13.png)
![preview](images.png/sonar14.png)
![preview](images.png/sonar15.png)

* Here we do not have the permissions for the user
![preview](images.png/sonar16.png)

* Let us give the permissions
![preview](images.png/sonar17.png)

* Now Let us create token for authentication
  
![preview](images.png/sonar18.png)
![preview](images.png/sonar19.png)
* Now copt the token and preserve somewhere, after this we cannot get the token back..we have to cteate new one. 

`Note: Keep a note of this token, we will be using it in jenkins to authenticate`

![preview](images.png/sonar20.png)

Let us now setup sonar-qube jenkins
------------------------------------
* Pre-requisite
    * install sonar qube Scanner plugin

![preview](images.png/sonar21.png)

* Now Add SonarQube Server to Jenkins
  -----------------------------------
* Now let us configure add sonar token
   
![preview](images.png/sonar23.png)
![preview](images.png/sonar22.png)
![preview](images.png/sonar24.png)

```
pipeline {
    agent{ label 'node1'}
      triggers{
      pollSCM('* * * * *')
      }

        stages {
            stage('vcs') {
               steps{
                  git url: 'https://github.com/shaiksohail11/spring-petclinic.git',
                  branch: 'main'
                }
            }
        
            stage('build') {
               steps{
                  sh "mvn package"
                }
        }
            stage('sonar analysis') {
               steps{
                  withSonarQubeEnv('SONAR_CLOUD') {
                  sh 'mvn clean package sonar:sonar -Dsonar.organization=jenkins12 -Dsonar.projectKey=jenkins12_jspc'
                     }
                }
        }
     }
        
}
```

![preview](images.png/sonar25.png)
![preview](images.png/sonar26.png)
![preview](images.png/sonar27.png)
![preview](images.png/sonar28.png)
![preview](images.png/sonar29.png)
* Now we can go and check in our Sonar cloud account, We have got our test results
  
![preview](images.png/sonar31.png)
![preview](images.png/sonar32.png)

* We can also check the test results directly in jenkins
![preview](images.png/sonar30.png)

------------------------------------------THE END--------------------------------------------

