---
layout: post
title:  "JPA Junit test"
author: Degan
categories: springboot 
comments: true
tags: spring boot
---

## JPA Junit test

- @DataJpaTest 
- H2 dependencies
- 설정하면 기존 DB와 상관없이 H2 메모리 DB에서 테스트 진행
- 실행 method에 @Rollback(false) 해야 실제 insert

```java
package com.degan.jpatest;

import static org.assertj.core.api.Assertions.assertThat;

import java.util.List;

import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.orm.jpa.DataJpaTest;
import org.springframework.data.domain.Page;
import org.springframework.data.domain.PageRequest;
import org.springframework.test.annotation.Rollback;

import com.degan.jpatest.domain.Post;
import com.degan.jpatest.repository.PostRepository;

// @SpringBootTest
@DataJpaTest
public class PostRepositoryTest {
    
    @Autowired
    PostRepository postRepository;

    @Test
    @Rollback(false)
    public void crudRepository() {
        Post post = new Post();
        post.setTitle("hello spring boot");
        assertThat(post.getId()).isNull();

        // When
        Post newPost = postRepository.save(post);

        // Then
        assertThat(newPost.getId()).isNotNull();

        // When
        List<Post> posts = postRepository.findAll();
        assertThat(posts.size()).isEqualTo(1);
        assertThat(posts.contains(newPost));

        // When
        Page<Post> page = postRepository.findAll(PageRequest.of(0, 10));
        assertThat(page.getTotalElements()).isEqualTo(1);
        assertThat(page.getNumber()).isEqualTo(0);

        // When
        postRepository.findByTitleContains("spring", PageRequest.of(0,10));
        assertThat(page.getTotalElements()).isEqualTo(1);
        assertThat(page.getNumber()).isEqualTo(0);
    }
}

```

---
## Reference

[스프링 데이터 JPA 강의](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EB%8D%B0%EC%9D%B4%ED%84%B0-jpa/dashboard)
