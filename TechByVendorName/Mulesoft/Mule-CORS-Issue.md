# CORS issue when using mule to serve micro services to frontend


When Angular call Restful API by Mule,

```
( cross domain HTTP request )
XMLHttpRequest cannot load http://localhost:8081/iserver/flows. No 'Access-Control-Allow-Origin' header is present on the requested resource. Origin 'http://xxxxxxx:8080' is therefore not allowed access.
```

Related document

http://blogs.mulesoft.com/dev/anypoint-platform-dev/cross-domain-rest-calls-using-cors/

## resolution
add response header

name: Access-Control-Allow-Origin

value: Put in the allowed client's http(s)://hostname:port
