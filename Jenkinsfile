node{
    try{
        def mavenHome
        def mavenCMD
        
        stage('Preparation'){
            echo "Preparing the Jenkins environment with required tools..."
            mavenHome = tool name: 'maven', type: 'maven'
            mavenCMD = "${mavenHome}/bin/mvn"
        }
        
        stage('git checkout'){
            echo "Checking out the code from git repository..."
            git 'https://github.com/anupabenny/devopsproject.git'
        }
        
        stage('Build, Test and Package'){
            echo "Building the application..."
            sh "${mavenCMD} -version"
        }
        
   }
   }
