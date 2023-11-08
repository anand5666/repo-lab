pipeline {
agent any

stages {
stage('chckout scm') {
steps {
checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/anand5666/javaproj.git']])
}
}
stage('Compiling and Running Test Cases') {
steps {
sh 'mvn clean'
sh 'mvn compile'
sh 'mvn test'
}
}
stage('Generating a Cucumber Reports') {
steps {
script {
// Run Cucumber tests and generate reports
sh 'mvn verify'
}
}
}
stage('Creating Package') {
steps {
sh 'mvn package'
}
}
stage('adding genrerate report'){
steps {
sh 'mvn verify'
}
}
stage('Install sonarqube cli') {
steps {
// Step to install SonarQube CLI
sh 'wget -O sonar-scanner.zip https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-5.0.1.3006-linux.zip'
sh 'unzip -o -q sonar-scanner.zip'
sh 'sudo rm -rf /opt/sonar-scanner'
sh 'sudo mv --force sonar-scanner-5.0.1.3006-linux /opt/sonar-scanner'
sh 'sudo sh -c \'echo "#/bin/bash \nexport PATH=\\\"$PATH:/opt/sonar-scanner/bin\\\"" >/etc/profile.d/sonar-scanner.sh\''
sh 'sudo chmod +x /opt/sonar-scanner/bin/sonar-scanner'
sh '. /etc/profile.d/sonar-scanner.sh'
}
}

stage('Analyzing Code Quality') {
steps {
// Step to analyze code quality with SonarQube
sh '/opt/sonar-scanner/bin/sonar-scanner -Dsonar.projectKey=anand5666_javaproj -Dsonar.organization=anand5666 -Dsonar.qualitygate.wait=true -Dsonar.qualitygate.timeout=300 -Dsonar.sources=src/main/java/ -Dsonar.java.binaries=target/classes -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=2d816cc847fa30cbce10c70c2ce39bb6e25a9109'
}
}
}
}

