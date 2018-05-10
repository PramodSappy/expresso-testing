node {

    stage 'Checkout'
        checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/PramodSappy/expresso-testing.git']]])
        sh "./gradlew clean"


    stage 'Assemble Android Test'
        sh "./gradlew assembleDebug"
        sh "./gradlew assembleDebugAndroidTest"


    stage 'Cloud Test Lab'
        sh "gcloud config set project expresso-testing"

        sh "gcloud auth login"

        sh " gcloud firebase test android run \
              --type robo \
              --app app-debug.apk \
              --device model=Nexus6,version=21,locale=en,orientation=portrait  \
              --device model=Nexus7,version=19,locale=fr,orientation=landscape \
              --timeout 90s"


    stage 'Build Release'
        sh "./gradlew assemble"


    stage 'Archive'
        step([$class: 'ArtifactArchiver', artifacts: 'app/build/outputs/apk/*.apk', fingerprint: true])
        step([$class: 'JUnitResultArchiver', testResults: 'app/build/test-results/**/TEST-*.xml'])

}