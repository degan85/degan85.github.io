---
layout: post
title: " ElasticSearch GET, POST, PUT, DELETE "
author: Degan
categories: ELK
tags:	elasticsearch get post put delete
comments: true
---

### GET

- index 생성 전
- 404 뜸

```
$ curl-XGET http://localhost:9200/classes
```

```
{"error":{"root_cause":[{"type":"index_not_found_exception","reason":"no such index","resource.type":"index_or_alias","resource.id":"classes","index_uuid":"_na_","index":"classes"}],"type":"index_not_found_exception","reason":"no such index","resource.type":"index_or_alias","resource.id":"classes","index_uuid":"_na_","index":"classes"},"status":404}
```

- 깔끔하게 보기

```
$ curl-XGET http://localhost:9200/classes?pretty
```

```
 {
  "error" : {
    "root_cause" : [
      {
        "type" : "index_not_found_exception",
        "reason" : "no such index",
        "resource.type" : "index_or_alias",
        "resource.id" : "classes",
        "index_uuid" : "_na_",
        "index" : "classes"
      }
    ],
    "type" : "index_not_found_exception",
    "reason" : "no such index",
    "resource.type" : "index_or_alias",
    "resource.id" : "classes",
    "index_uuid" : "_na_",
    "index" : "classes"
  },
  "status" : 404
}
```

### PUT

``` 
$ curl -XPUT htp://localhost:9200/classes?pretty
```
```
{
  "acknowledged" : true,
  "shards_acknowledged" : true,
  "index" : "classes"
}
```

- 다시 확인

```
$ curl -XGET htp://localhost:9200/classes?pretty
```

```
{
  "classes" : {
    "aliases" : { },
    "mappings" : { },
    "settings" : {
      "index" : {
        "creation_date" : "1519799319664",
        "number_of_shards" : "5",
        "number_of_replicas" : "1",
        "uuid" : "So7vmP40SVe_v_r6InVeCA",
        "version" : {
          "created" : "6020299"
        },
        "provided_name" : "classes"
      }
    }
  }
}
```

### DELETE

```
$ curl -XDELETE http://localhost:9200/classes
```

```
{"acknowledged":true}
```

### POST

- cteate

```
$ curl --header "content-type: application/JSON" 
-XPOST localhost:9200/classes/class/1/ -d '
{"title":"A", "professor":"J"}'
```

- 확인

```
$ curl -XGET http://localhost:9200/classes/class/1?pretty
```

```
{
  "_index" : "classes",
  "_type" : "class",
  "_id" : "1",
  "_version" : 1,
  "found" : true,
  "_source" : {
    "title" : "A",
    "professor" : "J"
  }
}
```

---

## Reference

[# ELK 스택 (ElasticSearch, Logstash, Kibana) 으로 데이터 분석](https://www.inflearn.com/course/elk-%EC%8A%A4%ED%83%9D-%EB%8D%B0%EC%9D%B4%ED%84%B0-%EB%B6%84%EC%84%9D/)
