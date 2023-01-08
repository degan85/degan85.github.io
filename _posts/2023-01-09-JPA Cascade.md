---
layout: post
title:  "JPA Cascade"
author: Degan
categories: spring boot 
comments: true
tags: spring boot
---

## JPA Cascade

- 부모 자식 관계일 경우 
- 릴레이션 객체의 [[JPA 상태]] 관리

```java
@Entity
public class Post {
    
    @Id @GeneratedValue
    private Long id;

    private String title;

    @OneToMany(mappedBy = "post")
    private Set<Comment> comments = new HashSet<>();

    public void addComment(Comment comment) {
        this.getComments().add(comment);
        comment.setPost(this);
    }

	// ...
```

```java
@Entity
public class Comment {

    @Id @GeneratedValue
    private Long id;

    private String comment;

    @ManyToOne
    private Post post;
```

- cascade 없는 경우

```java
Post post = new Post();
post.setTitle("Spring Data JPA ...");

Comment comment = new Comment();
comment.setComment("aaaaa");
post.addComment(comment);

Comment comment1 = new Comment();
comment1.setComment("soon");
post.addComment(comment1);

Session session = entityManager.unwrap(Session.class);
session.save(post);

// cascade가 안되있어서
// comment가 저장되지 않음
```

- cascade(PERSIST) 설정

```java
// Post.java

@OneToMany(mappedBy = "post", cascade = CascadeType.PERSIST)
private Set<Comment> comments = new HashSet<>();
```

- cascade(PERSIST) 진행
	- session.save() 실행
	- Post가 Persistent 상태가 됨
	- cascade로 인해 Comment도 Persistent 상태가 됨
	- post, comment insert

- cascade(REMOVE) 설정

```java
// Post.java

@OneToMany(mappedBy = "post", cascade = {CascadeType.PERSIST, CascadeType.REMOVE})
private Set<Comment> comments = new HashSet<>();
```

```java
Session session = entityManager.unwrap(Session.class);
Post post = session.get(Post.class, 1l);

session.delete(post);
```

- cascade(REMOVE) 진행
	- session.delete() 실행
	- Post가 Removed 상태가 됨
	- cascade로 인해 Comment도 Removed 상태가 됨
	- post, comment delete

- 대부분 cascadeType.ALL 사용

---
## Reference

[[스프링 데이터 JPA 강의]]

