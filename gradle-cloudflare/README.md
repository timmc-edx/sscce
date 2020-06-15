Gradle bootstrapping is blocked by Cloudflare, which rejects CLI
clients such as the gradle wrapper and curl:

https://github.com/gradle/gradle/issues/13433

When I try to run `./gradlew test` I get the following:

```
Downloading http://services.gradle.org/distributions/gradle-2.4-all.zip

Exception in thread "main" java.lang.RuntimeException: java.io.IOException: Server returned HTTP response code: 403 for URL: http://services.gradle.org/distributions/gradle-2.4-all.zip
	at org.gradle.wrapper.ExclusiveFileAccessManager.access(ExclusiveFileAccessManager.java:78)
	at org.gradle.wrapper.Install.createDist(Install.java:44)
	at org.gradle.wrapper.WrapperExecutor.execute(WrapperExecutor.java:126)
	at org.gradle.wrapper.GradleWrapperMain.main(GradleWrapperMain.java:58)
Caused by: java.io.IOException: Server returned HTTP response code: 403 for URL: http://services.gradle.org/distributions/gradle-2.4-all.zip
	at sun.net.www.protocol.http.HttpURLConnection.getInputStream0(HttpURLConnection.java:1900)
	at sun.net.www.protocol.http.HttpURLConnection.getInputStream(HttpURLConnection.java:1498)
	at org.gradle.wrapper.Download.downloadInternal(Download.java:56)
	at org.gradle.wrapper.Download.download(Download.java:42)
	at org.gradle.wrapper.Install$1.call(Install.java:57)
	at org.gradle.wrapper.Install$1.call(Install.java:44)
	at org.gradle.wrapper.ExclusiveFileAccessManager.access(ExclusiveFileAccessManager.java:65)
	... 3 more
```

If I open that URL in Firefox I get a ZIP file.

If I try to run
`curl -i http://services.gradle.org/distributions/gradle-2.4-all.zip`
I get a 403 Forbidden from Cloudflare:

```
HTTP/1.1 403 Forbidden
Date: Mon, 15 Jun 2020 16:50:30 GMT
Content-Type: text/plain; charset=UTF-8
Content-Length: 16
Connection: keep-alive
X-Frame-Options: SAMEORIGIN
Cache-Control: private, max-age=0, no-store, no-cache, must-revalidate, post-check=0, pre-check=0
Expires: Thu, 01 Jan 1970 00:00:01 GMT
Set-Cookie: __cfduid=de3e9c2bf19e2aeea9c6604948a3990be1592239830; expires=Wed, 15-Jul-20 16:50:30 GMT; path=/; domain=.gradle.org; HttpOnly; SameSite=Lax; Secure
cf-request-id: 035a7ccdd40000ebecac9da200000001
Server: cloudflare
CF-RAY: 5a3dca5c8a8bebec-BOS

error code: 1020
```
