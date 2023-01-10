---
layout: post
title:  "Controller Junit Test"
author: Degan
categories: springboot 
comments: true
tags: spring boot
---

## Controller Junit Test

- vscode에서 assertThat, test 관련 import 잘 안됨
	- 확인
	
- andDo(print())로 확인하면서 작성하면 편함

```java
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultHandlers.print;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.content;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.jsonPath;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.AutoConfigureMockMvc;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.web.servlet.MockMvc;

import com.degan.jpatest.domain.Post;
import com.degan.jpatest.repository.PostRepository;

@SpringBootTest
@AutoConfigureMockMvc
public class PostControllerTest {

    @Autowired
    MockMvc mockMvc;

    @Autowired
    PostRepository postRepository;

    @Test
    public void getPost() throws Exception {
        Post post = new Post();
        post.setTitle("jpa");
        postRepository.save(post);


        mockMvc.perform(get("/posts/1"))
            .andDo(print())
            .andExpect(status().isOk())
            .andExpect(content().string("jpa"));
    }

    @Test
    public void getUseConverterPost() throws Exception {
        Post post = new Post();
        post.setTitle("jpa2");
        postRepository.save(post);


        mockMvc.perform(get("/cposts/1"))
            .andDo(print())
            .andExpect(status().isOk())
            .andExpect(content().string("jpa2"));
    }
    
    @Test
    public void getPosts() throws Exception {
        Post post = new Post();
        post.setTitle("jpa");
        postRepository.save(post);

        mockMvc.perform(get("/posts/")
                    .param("page", "0")
                    .param("size", "10")
                    .param("sort", "title"))
                .andDo(print())
                .andExpect(status().isOk())
                .andExpect(jsonPath("$.content[0].title").value("jpa"));
    }
}
```

---
## Reference


- [스프링 데이터 JPA 강의](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EB%8D%B0%EC%9D%B4%ED%84%B0-jpa/dashboard)
