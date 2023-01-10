---
layout: post
title:  "JPA DomainClassConverter"
author: Degan
categories: springboot 
comments: true
tags: spring boot
---

## JPA DomainClassConverter

- DomainClassConverter.class
	- ToEntityConverter.class


```java

@GetMapping("/posts/{id}")
public String getAPost(@PathVariable Long id) {
	Optional<Post> byId = postRepository.findById(id);
	Post post = byId.get();
	return post.getTitle();
}

// DomainClassConverter
// ToEntityConverter에서 findById 동작(위와 동일)
// id가 없으면 converted to null error 발생
@GetMapping("/posts/{id}")
public String getApost(@PathVariable("id") Post post) {
	return post.getTitle();
}
```
---
## Reference


