A hello world program to print greeting message based on time.

Note Jenkinsfile ngoducquyet/hello-world-greeting
Using the SonarQube scanner for Maven
<properties>
<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
<sonar.language>java</sonar.language>
</properties>

to spawn a Docker container (Jenkins slave) we need to write a code
block for node with the label as docker:
node('docker') {
}

sh 'mvn clean verify -DskipITs=true';
junit '**/target/surefire-reports/TEST-*.xml'
archive 'target/*.jar'
Where -DskipITs=true is the option to skip the integration test and perform only the
build and unit test.
The junit '**/target/surefire-reports/TEST-*.xml' command enables Jenkins to
publish JUnit unit test reports on the Jenkins pipeline page. **/target/surefirereports/TEST-*.xml is the directory location where the unit test reports are generated.

sh 'mvn clean verify sonar:sonar -Dsonar.projectName=example-project
-Dsonar.projectKey=example-project -Dsonar.projectVersion=$BUILD_NUMBER';
The -Dsonar.projectName=example-project option is the option to pass the
SonarQube project name. In this way, all our results will be visible under the
projectName=example-project that we created in the previous chapter.

Similarly, the -Dsonar.projectKey=example-project option allows the SonarQube
scanner for the Maven utility to confirm the projectKey=example-project with
SonarQube.

The -Dsonar.projectVersion=$BUILD_NUMBER option allows us to attach the Jenkins
build number with every analysis that we perform and upload to
SonarQube. $BUILD_NUMBER is the Jenkins environment variable for the build number.

sh 'mvn clean verify -Dsurefire.skip=true';
junit '**/target/failsafe-reports/TEST-*.xml'
archive 'target/*.jar'

Where -Dsurefire.skip=true is the option to skip unit testing and perform only the
integration testing.
The junit '**/target/failsafe-reports/TEST-*.xml' command enables Jenkins to
publish JUnit unit test reports on the Jenkins pipeline page. **/target/failsafereports/TEST-*.xml is the directory location where the integration test reports are
generated.
