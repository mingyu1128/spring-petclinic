pipeline {
  agent any

  tools {
    jdk "jdk17"
    maven "M3"
  }
  stages {
    stage () {
      steps {
        echo "Git clone"
        git url: 'https://github.com/mingyu1128/spring-petclinic.git',
          branch: 'efficient-webjars'
      }
    }
  }
}
