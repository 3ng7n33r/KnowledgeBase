# URL
Uniform Resource Locators are the mechanism used by browsers to retrieve any published resource on the web.
A URL is composed of multiple parts, some mandatory, some optional. They can be absolute or relative.

## Anatomy
```html
http://www.example.com:80/path/to/myfile.html?key1=value1&key2=value2#SomewhereInTheDocument
```
### Protocol ("http")
Tells the browser which protocol to use. Most common are [http](https://3ng7n33r.github.io/KnowledgeBase/webdevelopment/web/http.html) and [https](https://3ng7n33r.github.io/KnowledgeBase/webdevelopment/web/https.html) but others like [mailto](https://3ng7n33r.github.io/KnowledgeBase/webdevelopment/web/email.html) , [data](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/Data_URIs) and [ftp](https://3ng7n33r.github.io/KnowledgeBase/webdevelopment/web/ftp.html) are possible.

### Domain Name ("www.example.com")
The domain name indicates which web server is being requested. If it is a string, a [DNS](https://3ng7n33r.github.io/KnowledgeBase/webdevelopment/web/dns.html)  server is contacted to translate it into an [IP](https://3ng7n33r.github.io/KnowledgeBase/webdevelopment/web/ip.html) address.
### Port (":80")
Indicates the gate where the web server should be accessed. If the standard gates are contacted (80 for HTTP, 443 for HTTPS) then it can be omitted.

### Path ("/path/to/myfile.html")
Is the path to the resource on the web server

### Parameters ("?key1=value1&key2=value2")
A list of key/value pairs provided to the web server, initiated with a question mark and separated with an ampersand. For example, they can be used to recall data from an [API](https://3ng7n33r.github.io/KnowledgeBase/webdevelopment/web/api.html) and are server specific. 
Further info [here](https://idratherbewriting.com/learnapidoc/docapis_doc_parameters.html).

### Anchor ("#SomewhereInTheDocument")
Initiated with a # (in this case called fragment identifier and not actually sent to the server with the request). The Anchor can be seen as a bookmark, either a spot in an HTML document or a timestamp in an video.

## Sources

 - [MDN](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_is_a_URL)


Any copyright is dedicated to the Public Domain. http://creativecommons.org/publicdomain/zero/1.0/

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEyNzU2MTc2ODcsNTI2OTI1NjM4LC0zOD
AxMTc1ODksMjA5NTkyODQ0NV19
-->