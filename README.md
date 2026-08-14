# mini-private-maven
A mini private maven service, use as a maven server, which can proxy, cache files, deploy local jars.   
It can serve as an alternative to software such as `Nexus` or `Artifactory` for Maven artifact management.

## Usage

### Maven Server

- Config config.json


```json
{
    "addr":"0.0.0.0",
    "port":8431,
    "cache_dir": "./storage",
    "listing":true,
    "protocol":"http",
    "dont_cache_snapshots":true,
    "https":{
        "cert":"./config/cert.pem",
        "key":"./config/cert-key.pem"
    },
    "auth":  [
    ],
    "repos": [
        "https://repo1.maven.org/maven2",
        "https://maven.aliyun.com/repository/central/",
        "https://repo2.maven.org/maven2/",
        "https://maven.aliyun.com/repository/public/"
    ]
}

```


- Start service


`node ./proxy.js ./config.json`


### Maven client

- Config `settings.xml` of maven


```xml
<?xml version="1.0" encoding="UTF-8"?>
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">
    <mirrors>
        <mirror>
            <id>maven3</id>
            <mirrorOf>*</mirrorOf>
            <name>3-proxy</name>
            <url>http://192.168.0.3:8431/</url>
        </mirror>
</settings>
```

- Use by mvn

Update dependency: 

`mvn clean dependency:resolve -U`

Local deploy:

`mvn deploy`

