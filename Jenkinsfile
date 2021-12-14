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
    def pageNum = 1
    String bottomTag, topTag, body = "", totalText
    topTag = '<!DOCTYPE html> <html> <head> <style> .flex{ display: flex; flex-wrap: wrap; justify-content: center;align-items: center;}.flex > div {text-align: center;padding:15px}img {display: inline-block;max-width: 100%;}</style></head> <h1 style="text-align:center; font-family: sans-serif;">Hello ReqRes members</h1> <div class = "flex">'
    // echo "${response}"
    while(pageNum) {    
        def response = httpRequest "https://reqres.in/api/users?page=$pageNum"
        JsonSlurper slurper = new JsonSlurper()
        Map parsedJson = slurper.parseText(response.content)
        if(parsedJson.data.isEmpty())
            break
        for(def x: parsedJson.data) {
            def name = x.first_name
            def email = x.email
            def image = x.avatar
            
            body += '<div>' + '<p><strong>' + name + '</strong></p>' + '<p>' + email + '</p>' + '<img src="' + image + '">' + "</div>" 
            // echo "$body"
        }
        pageNum++
    }
    bottomTag = "</div> </body> </html>"
    totalText = topTag + body + bottomTag 
    echo "$totalText"
    def newFile = new File("${env.WORKSPACE}/index.html")
    newFile.createNewFile()
    newFile.text = totalText
    // openHtml()
}
