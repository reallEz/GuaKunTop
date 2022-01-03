// COPY OF: https://raw.githubusercontent.com/blindpirate/xiedaimala-springboot/master/Jenkinsfile
String buildNumber = env.BUILD_NUMBER;
String timestamp = new Date().format('yyyyMMddHHmmss');
String projectName = env.JOB_NAME.split(/\//)[0];
// e.g awesome-project/release/RELEASE-1.0.0
String branchName = env.JOB_NAME.split(/\//)[1..-1].join(/\//);

println("${buildNumber} ${timestamp} ${projectName}");

String version = "${buildNumber}-${timestamp}-${projectName}";

node {
    checkout scm;

    if(params.BuildType=='Rollback') {
        return rollback()
    } else if(params.BuildType=='Normal'){
        return normalCIBuild(version)
    } else if(branchName == 'main'){
        setScmPollStrategyAndBuildTypes(['Normal', 'Rollback']);
    }
}

def normalCIBuild(String version) {
    stage('deploy')

    input 'deploy?'

    deployVersion(version)
}

def deployVersion(String version) {
    sh "cp ./index.html /home/gua/Statics/GuakunTop"
}

def setScmPollStrategyAndBuildTypes(List buildTypes) {
    def propertiesArray = [
            parameters([choice(choices: buildTypes.join('\n'), description: '', name: 'BuildType')]),
            pipelineTriggers([[$class: "SCMTrigger", scmpoll_spec: "* * * * *"]])
    ];
    properties(propertiesArray);
}

def rollback() {
    deployVersion(rollbackVersion)
}