
**CORS Anywhere** is a NodeJS proxy which adds CORS headers to the proxied request.

The url to proxy is literally taken from the path, validated and proxied. The protocol
part of the proxied URI is optional, and defaults to "http". If port 443 is specified,
the protocol defaults to "https".

This package does not put any restrictions on the http methods or headers, except for
cookies. Requesting [user credentials](http://www.w3.org/TR/cors/#user-credentials) is disallowed.
The app can be configured to require a header for proxying a request, for example to avoid
a direct visit from the browser.

## Example

####  No Header required for easy use of the url so that in places like HTML img tag you can send request without thinking of setting headers
```javascript
// Listen on a specific host via the HOST environment variable
var host = process.env.HOST || '0.0.0.0';
// Listen on a specific port via the PORT environment variable
var port = process.env.PORT || 8080;

var cors_proxy = require('cors-anywhere');
cors_proxy.createServer({
    originWhitelist: [], // Allow all origins
    removeHeaders: ['cookie', 'cookie2']
}).listen(port, host, function() {
    console.log('Running CORS Anywhere on ' + host + ':' + port);
});

```
Request examples:

* `http://localhost:8080/http://google.com/` - Google.com with CORS headers
* `http://localhost:8080/google.com` - Same as previous.
* `http://localhost:8080/google.com:443` - Proxies `https://google.com/`
* `http://localhost:8080/` - Shows usage text, as defined in `lib/help.txt`
* `http://localhost:8080/favicon.ico` - Replies 404 Not found

Live examples:

* https://corsapproval.herokuapp.com/https://www.google.com/webhp - This demo shows how to use the API.

### Demo server

A public demo of CORS Anywhere is available at https://cors-anywhere.herokuapp.com. This server is
only provided so that you can easily and quickly try out CORS Anywhere. To ensure that the service
stays available to everyone, the number of requests per period is limited, except for requests from
some explicitly whitelisted origins.

**Note: as of February 2021, access to the demo server requires an opt-in**,
see: https://github.com/Rob--W/cors-anywhere/issues/301

If you expect lots of traffic, please host your own instance of CORS Anywhere, and make sure that
the CORS Anywhere server only whitelists your site to prevent others from using your instance of
CORS Anywhere as an open proxy.

For instance, to run a CORS Anywhere server that accepts any request from some example.com sites on
port 8080, use:
```
export PORT=8080
export CORSANYWHERE_WHITELIST=https://example.com,http://example.com,http://example.com:8080
node server.js
```

This application can immediately be run on Heroku, see https://devcenter.heroku.com/articles/nodejs
for instructions. Note that their [Acceptable Use Policy](https://www.heroku.com/policy/aup) forbids
the use of Heroku for operating an open proxy, so make sure that you either enforce a whitelist as
shown above, or severly rate-limit the number of requests.

For example, to blacklist abuse.example.com and rate-limit everything to 50 requests per 3 minutes,
except for my.example.com and my2.example.com (which may be unlimited), use:

```
export PORT=8080
export CORSANYWHERE_BLACKLIST=https://abuse.example.com,http://abuse.example.com
export CORSANYWHERE_RATELIMIT='50 3 my.example.com my2.example.com'
node server.js
```



