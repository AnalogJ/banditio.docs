
![](https://github.com/AnalogJ/banditio.docs/blob/master/bandito structure.png)

# Components
- Frontend: Chrome Devtools (+ Websocket client)
- Engine: Websocket Server + Nginx Reverse Proxy
- Container: MITMProxy + Websocket client

# Frontend
The frontend is basically a static website that displays the Chrome devtools in an iframe. 
This iframe url is modified once a proxy container is started, and points to the websocket server running on the Engine server.

## Testing

- window 1

	/Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome --remote-debugging-port=9222 --no-first-run --user-data-dir=/tmp/chrome-dev-profile http://localhost:9222 http://chromium.org
	http://localhost:9222/devtools/inspector.html?ws=proxy.bandit.io:9000/ws/all
	# then click on the chromium tab
	
- window 2

	http://localhost:9222/json
	find websocketdebuggerurl
	chrome-devtools://devtools/bundled/devtools.html?ws=localhost:9222/devtools/page/97EB937F-B13F-416B-A2CD-98A8453B4314
	reload window 1
	find websocket, view connection informaiton
	
# Engine
The engine is made up of 3 different applications, a websocket server, a webserver, and a nginx server. 

## Testing

- The websocket server can be tested by visiting http://www.websocket.org/echo.html and entering a url of ws://websocket.bandit.io:9000/ws/**{containerid}**
- The webserver can be tested by visiting http://websocket.bandit.io:9000/json
- The nginx server can be tested by running the following command: curl -L --proxy http://**{containerid}**.bandit.io http://www.cnn.com

# MITM Proxy Containers
The MITM Proxy containers are spun up on demand, and listen for HTTP_PROXY requests from NGINX. 

## Testing
The MITM Proxy containers are not publically accessible. They can only be accessed via the Engine server. 



# References
- https://github.com/ChromeDevTools/devtools-frontend
- chrome-devtools://devtools/bundled/devtools.html
- https://github.com/c4milo/node-webkit-agent/issues/32
- https://developer.chrome.com/devtools/docs/integrating
- https://developer.chrome.com/devtools/docs/protocol/1.1/network#events
- https://github.com/mirumee/chromedebug/blob/master/chromedebug/server.py
- https://github.com/square/PonyDebugger/blob/master/ponyd/gateway.py
- https://github.com/square/PonyDebugger/blob/master/ponyd/downloader.py
- https://chromium.googlesource.com/chromium/blink/+/master/Source/devtools/front_end/
- https://chrome-devtools-frontend.appspot.com/
- https://chrome-devtools-frontend.appspot.com/serve_rev/@161986/devtools.html
- https://chrome-devtools-frontend.appspot.com/serve_rev/@195284/devtools.html
- https://stackoverflow.com/questions/28430479/using-google-chrome-remote-debugging-protocol
- https://github.com/mitmproxy/mitmproxy/blob/master/examples/har_extractor.py
- http://www.softwareishard.com/har/viewer/?inputUrl=http://www.janodvarko.cz/har/viewer/examples/inline-scripts-block.harp
- https://chromium.googlesource.com/chromium/blink/+archive/9c1f3db8fdeaaf3e74f5dc0f6e71cba556569ad2.tar.gz
- https://chromium.googlesource.com/chromium/blink/+archive/9c1f3db8fdeaaf3e74f5dc0f6e71cba556569ad2/Source/devtools/front_end.tar.gz
- http://blog.webernetz.net/2014/01/22/at-a-glance-http-proxy-packets-vs-normal-http-packets/