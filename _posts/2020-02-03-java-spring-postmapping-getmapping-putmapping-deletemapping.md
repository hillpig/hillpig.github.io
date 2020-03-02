---
layout: post
title: Spring boot - @PostMapping @GetMapping @PutMapping @DeleteMapping
date: 2020-02-05
Author: 山猪
tags: [Java, Spring Boot]
comments: true
---
![img](https://i2.wp.com/springframework.guru/wp-content/uploads/2017/09/mvc.png?w=800&ssl=1)

<!-- more -->


-    @PostMapping – Handle HTTP POST Requests
-    @GetMapping – Handle HTTP Get Requests
-    @PutMapping – Handle HTTP Put Requests
-    @DeleteMapping – Handle HTTP Delete Requests

 Additionally to the above-mentioned shortcut annotations, you can use the @RequestMapping annotation which you will most probably find in most of the Spring MVC REST Web Service projects. You can use this single @RequestMapping annotation to map any request to a specific method.

**A few HTTP GET request mapping examples:**

```java
    @RequestMapping(method=RequestMethod.GET)
```

```java
    @RequestMapping(method=RequestMethod.GET, path = "/{id}", produces = MediaType.APPLICATION_JSON_VALUE)
```

```java
    @RequestMapping(method=RequestMethod.GET, produces = {MediaType.APPLICATION_JSON_VALUE, MediaType.APPLICATION_XML_VALUE})
```

```java
    @RequestMapping(method=RequestMethod.GET, path = "/{id}", produces = {MediaType.APPLICATION_JSON_VALUE, MediaType.APPLICATION_XML_VALUE})
```

**To map HTTP Post request to a method, we just need to change the Request Method type:**

```java
    @RequestMapping(method=RequestMethod.POST, consumes=MediaType.APPLICATION_JSON_VALUE, produces = MediaType.APPLICATION_JSON_VALUE)
```

```java
    @RequestMapping(method=RequestMethod.POST, path = "/{id}", consumes=MediaType.APPLICATION_JSON_VALUE, produces = MediaType.APPLICATION_JSON_VALUE)
```

Please note that if you need your Web Service endpoint to be able to return information in either XML or JSON format. Depending on the value of the Accept Header. Then you need you use the curly brackets {} and list the supported media types like so:

```java
    @RequestMapping(method=RequestMethod.POST, path = "/{id}", consumes = {MediaType.APPLICATION_JSON_VALUE, MediaType.APPLICATION_XML_VALUE}, produces = {MediaType.APPLICATION_JSON_VALUE, MediaType.APPLICATION_XML_VALUE})
```

Note: Note that if you want your Web Service endpoint to return MediaType.APPLICATION_XML_VALUE then your HTTP Request must contain the Accept header with a value application/xml like in the curl example below:

```console
    curl -X GET \
    http://localhost:8080/users/Ow8EG5GAvkCa9kEHGqLTQpR32zSmhd /
    -H 'accept: application/xml' /
    -H 'authorization: Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJ0ZXN0QHRlc3QuY29tIiwiZXhwIjoxNTIzMDMwNTg4fQ.rnOqIEpmh6xQYBPkcRHp9DOnT8M5a7o9De6a7ZE7z2nBKfNpgXNrXgFQnlUi01CnltOP-iXyprsGhB4wvkafcg'
```
