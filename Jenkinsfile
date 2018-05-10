node {

    stage 'Checkout'
        checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/PramodSappy/expresso-testing.git']]])
        sh "./gradlew clean"


    stage 'Assemble Android Test'
        sh "./gradlew assembleDebugAndroidTest"


    stage 'Cloud Test Lab'
        sh "gcloud auth activate-service-account --key-file =./expresso-testing-7bb5e3c13011.json"

        sh " gcloud firebase test android run \
              --type instrumentation \
              --app **/build/outputs/apk/debug/*.apk \
              --test **/build/outputs/apk/androidTest/debug/*.apk \
              --device model=Nexus6,version=21,locale=en,orientation=portrait  \
              --device model=Nexus7,version=19,locale=fr,orientation=landscape"

    stage 'Build Release'
        sh "./gradlew assemble"

}
