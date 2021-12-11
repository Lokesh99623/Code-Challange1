import groovy.json.JsonSlurper
pipeline {
    agent any 
        stages {
            stage('Build') {
                steps {
                    script {
                        fetchData()
                    }
                }
            }
        }
}
def fetchData() {
    def response = httpRequest "https://reqres.in/api/users?page=2"
    String bottomTag, topTag, body = "", totalText
    topTag = "<!DOCTYPE html> <html> <body> <h1>ReqRes members <hr></h1>"
    // echo "${response}"
    JsonSlurper slurper = new JsonSlurper()
    Map parsedJson = slurper.parseText(response.content)
    for(def x: parsedJson.data) {
        def name = x.first_name
        def email = x.email
        def image = x.avatar
        
        body += '<p>' + name + '</p>' + '<br>' + '<p>' + email + '</p>' + '<br>' + '<img src="' + image + '"' + 'width="50"' + 'height="60">' + '<hr>'
        // echo "$body"
    }
    bottomTag = "</body> </html>"
    totalText = topTag + body + bottomTag 
    // echo "$totalText"
    def newFile = new File("${env.WORKSPACE}/index.html")
    newFile.createNewFile()
    newFile.text = totalText
    // openHtml()
}

