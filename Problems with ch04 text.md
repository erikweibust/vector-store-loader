# Problems with Ch 4

## Section 4.3.1

1. Figure 4.3
it shows using Chroma but I believe it should be qdrant

2. Also had to change the compose.yaml in board-game-buddy to expose the port at 6334

```yaml
services:
  qdrant:
    image: 'qdrant/qdrant:latest'
    ports:
      - '6334:6334'
```

### Note: Not sure if this warning will cause an issue

```java
2025-08-19T17:51:16.709-05:00  WARN 40775 --- [vector-store-loader] [main] io.qdrant.client.QdrantGrpcClient        : Qdrant client version 1.13.0 is incompatible with server version 1.15.3. Major versions should match and minor version difference must not exceed 1. Set checkCompatibility=false to skip version check.
```

## Section 4.3.2

Not sure if I need both doc reader dependencies?

When adding docReader bean I got a number of errors related to missing imports; easy fix

### Listing 4.1 has many issues

1. Need to define loggers
2. Prompt template is not defined

I used the following:
```java
@Value("classpath:/promptTemplates/nameOfTheGame.st")
Resource nameOfTheGameTemplateResource;
```

3. The package name is wrong in the code for GameTitle.

Should be:
```java
package com.example.vectorstoreloader;
```

At the end of the section when I started up the vector-store-loader and copid a pdf to /var/dropoff...

1. Never saw the following in my log file:
```
TextSplitter                 : Splitting up document into 2 chunks.
GameRulesLoaderApplication : Determined game title to be Burger Battle
GameRulesLoaderApplication : Writing 2 documents to vector store.
GameRulesLoaderApplication : 2 documents have been written to vector store.
```

2. And got errors when running the http get request. First off, you listed the http call with port 8000, but
I think based on the config of "port=0" that the port is randomly assigned. When I can the command as-is I got
the following error.

```bash
~/Downloads via  v24.3.0
x--->  http :8080/api/v1/collections
HTTP/1.1 404
Connection: keep-alive
Content-Type: application/json
Date: Wed, 20 Aug 2025 03:37:38 GMT
Keep-Alive: timeout=60
Transfer-Encoding: chunked
Vary: Origin
Vary: Access-Control-Request-Method
Vary: Access-Control-Request-Headers

{
    "error": "Not Found",
    "path": "/api/v1/collections",
    "status": 404,
    "timestamp": "2025-08-20T03:37:38.601+00:00"
}
```

When I checked my Boot logs and found Tomcat started on port XXX (for me it said 50530) and used that I got 
the following. Still not working. You can check the startup.log in my ./logs dir

```bash
~/Downloads via  v24.3.0 took 30s
❯ http :50530/api/v1/collections
HTTP/1.1 503
Connection: close
Content-Type: application/json
Date: Wed, 20 Aug 2025 00:38:48 GMT
Transfer-Encoding: chunked

{
    "error": "Service Unavailable",
    "path": "/api/v1/collections",
    "status": 503,
    "timestamp": "2025-08-20T00:38:48.116+00:00"
}
```
