---
layout: default
categories: openmaint
tags: installation
title: Openmaint notes
---

# Openmaint notes

## Version matrix

Cmdbuild 3.3 based:

| Openmaint   | Java | Tomcat | Postgres | Postgis | BIMServer |
| ----------- | ---- | ------ | -------- | ------- | --------- |
| 2.2-3.3.2-w | 11   | 9.0.30 | 10       | 2.5     | 1.5.138   |
| 2.2-3.3.3   | 11   | 9.0.30 | 10       | 2.5     | 1.5.138   |

CMDbuild 3.4 based:

| Openmaint   | Java | Tomcat | Postgres | Postgis | BIMServer |
| ----------- | ---- | ------ | -------- | ------- | --------- |
| 2.2-3.4.1   | 17   | 9.0.53 | 12       | 3.2.1   | 1.5.138   |
| 2.3-3.4.1-d | 17   | 9.0.65 | 12       | 3.2.1   | 1.5.138   |
| 2.3-3.4.2   | 17   | 9.0.75 | 12       | 3.3     | 1.5.138   |


## Docker requirements:

### 2.2-3.3.2-w

Dockerfile excerpt:

```Dockerfile
FROM tomcat:9.0.30-jdk11-openjdk

ENV CMDBUILD_URL https://sourceforge.net/projects/openmaint/files/2.2/openmaint-2.2-3.3.2-w.war/download
```

Postgis from compose:

```yaml
image: postgis/postgis:10-2.5-alpine
```


### 2.2-3.4.1

Dockerfile excerpt:

```Dockerfile
FROM tomcat:9.0.53-jdk17-temurin

ENV CMDBUILD_URL https://sourceforge.net/projects/openmaint/files/2.2/Core%20updates/openmaint-2.2-3.4.1/openmaint-2.2-3.4.1.war/download
```

Postgis from compose:

```yaml
image: postgis/postgis:12-3.2-alpine
```

### 2.3-3.4.1-d

Dockerfile excerpt:

```Dockerfile
FROM tomcat:9.0.71-jdk17-temurin

ENV CMDBUILD_URL https://sourceforge.net/projects/openmaint/files/2.3/openmaint-2.3-3.4.1-d.war/download
```

Postgis from compose:

```yaml
image: postgis/postgis:12-3.2-alpine
```

### 2.3-3.4.2

Dockerfile excerpt:

```Dockerfile
FROM tomcat:9.0.75-jdk17-temurin

ENV CMDBUILD_URL https://sourceforge.net/projects/openmaint/files/2.3/Core%20updates/openmaint-2.3-3.4.2/openmaint-2.3-3.4.2.war/download
```

Postgis from compose:

```yaml
image: postgis/postgis:12-3.3-alpine
```