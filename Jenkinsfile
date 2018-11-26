pipeline {
    agent {
        node {
            label 'darwin-fast'
            // customWorkspace '/Users/jenkins/jenkins/workspace/temp/'
        }
    }
    environment {
        npm_config_brave_google_api_key     = credentials('npm_config_brave_google_api_key')
        REFERRAL_API_KEY = credentials('REFERRAL_API_KEY')
    }
    stages {
        stage('install') {
            steps {
                sh 'npm install'
            }
        }
        stage('init') {
            steps {
                sh 'npm run init'
            }
        }
        stage('build') {
            steps {
                sh """
                    export npm_config_brave_google_api_endpoint="https://location.services.mozilla.com/v1/geolocate?key="
                    export npm_config_google_api_endpoint="safebrowsing.brave.com"
                    export npm_config_google_api_key="dummytoken"

                    npm config set brave_google_api_endpoint "https://location.services.mozilla.com/v1/geolocate?key="
                    npm config set brave_google_api_key ${npm_config_brave_google_api_key}
                    npm config set google_api_endpoint "safebrowsing.brave.com"
                    npm config set google_api_key "dummytoken"

                    npm config set brave_referrals_api_key=${REFERRAL_API_KEY}

                    npm run build Release --debug_build=false --official_build=true --channel=dev
                """
            }
        }
        stage('test-security') {
            steps {
                sh 'npm run test-security -- --output_path="src/out/Release/Brave\ Browser\ Dev.app/Contents/MacOS/Brave\ Browser\ Dev"'
            }
        }
    }
}
