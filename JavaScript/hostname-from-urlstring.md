# 如何从 URL 字符串中提取主机名

```js
function getHostName(url) {
    var match = url.match(/:\/\/(www[0-9]?\.)?(.[^/:]+)/i);
    if (match != null && match.length > 2 && typeof match[2] === 'string' && match[2].length > 0) {
    return match[2];
    }
    else {
        return null;
    }
}
```

[Parsing Hostname and Domain from a Url with Javascript](http://www.primaryobjects.com/2012/11/19/parsing-hostname-and-domain-from-a-url-with-javascript/)

