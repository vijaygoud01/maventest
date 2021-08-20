node(){
    stage("git version control"){
        checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/vijaygoud01/maventest.git']]])
    }
    stage("code_analasys"){
        sh '''mvn sonar:sonar \\
  -Dsonar.projectKey=test \\
  -Dsonar.host.url=http://18.119.110.234:9000 \\
  -Dsonar.login=fd4edfb4e975304894a1cb9849f4f89cc167a4c7'''
    }
    stage("build"){
        sh 'mvn package'
    }
    stage("artifactory"){
       sh '''cp webapp/target/webapp.war webapp/target/webapp-$BUILD_ID.war
curl -u admin:Jaisriram@123 -T webapp/target/webapp-$BUILD_ID.war "http://18.119.110.234:8081/artifactory/example-repo-local/"'''
    }
    stage("deployment"){
        deploy adapters: [tomcat8(credentialsId: 'tomcat', path: '', url: 'http://3.135.221.84:8080/')], contextPath: 'test', war: '**/*.war'
    }
}
