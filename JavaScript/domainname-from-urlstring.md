# 如何从 URL 字符串中提取域名

```js
function getDomain(url) {
    var hostName = getHostName(url);
    var domain = hostName;
    
    if (hostName != null) {
        var parts = hostName.split('.').reverse();
        
        if (parts != null && parts.length > 1) {
            domain = parts[1] + '.' + parts[0];
                
            if (hostName.toLowerCase().indexOf('.co.uk') != -1 && parts.length > 2) {
              domain = parts[2] + '.' + domain;
            }
        }
    }
    
    return domain;
}
```

[Parsing Hostname and Domain from a Url with Javascript](http://www.primaryobjects.com/2012/11/19/parsing-hostname-and-domain-from-a-url-with-javascript/)

