---
layout: post
title: "[실전 스프링 부트 워크북] 스프링 부트 테스트 "
author: Degan
categories: springboot
tags:	spring boot junit test web mock
comments: true
---

## Test

- spring-boot-starter-test만 있으면 됨
	- JUnit, Objenesis, Hamcrest(Macher), Mockito 등 포함
- **@RunWith(SpringRunner.class)**
	- JUnit 라이브러리인 RunWith는 JUnit 내장 실행기 대신
	- SpringJUnit4ClassRunner.class를 상속 받은 
	- SpringRunner.class를 참조하여 테스트 실행
	- BlockJUnit4ClassRunner를 커스텀 확장한 클래스
- **@Test**
	- 테스트 할 여러 메서드에 붙여도 되지만
	- 순서가 보장되지 않음
	- @FixMethodOrder 같은 애너테이션을 붙여서 순서 지정
- **@SpringBootTest(WebEnvironment=WebEnvironment.RANDOM_PORT)**
	- 웹 환경을 모킹(mocking)하여 테스트 수행
	
- 햄크레스트 라이브러리와 JsonPath를 함께쓰면 REST형 API를 유연하게 테스트할 수 있음

```java
@RunWith(SpringJUnit4ClassRunner.class)
//@SpringApplicationConfiguration(classes = SpringBootWebApplication.class)
@SpringBootTest(webEnvironment = WebEnvironment.RANDOM_PORT)
//@WebAppConfiguration
@FixMethodOrder(MethodSorters.NAME_ASCENDING)

public class SpringBootWebApplicationTests {

  private final String SPRING_BOOT_MATCH = "스프링 부트";
  private final String CLOUD_MATCH = "부트";
  @SuppressWarnings("rawtypes")
  private HttpMessageConverter mappingJackson2HttpMessageConverter;
  private MediaType contentType = new MediaType(MediaType.APPLICATION_JSON.getType(), MediaType.APPLICATION_JSON.getSubtype(), Charset.forName("utf-8"));
  private MockMvc mockMvc;

  @Autowired
  private WebApplicationContext webApplicationContext;

  @Autowired
  void setConverters(HttpMessageConverter<?>[] converters) {
    this.mappingJackson2HttpMessageConverter = Arrays.asList(converters).stream().filter(
											 converter -> converter instanceof MappingJackson2HttpMessageConverter).
      findAny().get();
  }

  @Before
  public void setup() throws Exception {
    this.mockMvc = webAppContextSetup(webApplicationContext).build();
  }

  @Test
  public void getAll() throws Exception {
    mockMvc.perform(get("/journal/all"))
      .andExpect(status().isOk())
      .andExpect(content().contentType(contentType))
      .andExpect(jsonPath("$",iterableWithSize(5)))
      .andExpect(jsonPath("$[0]['title']",containsString(SPRING_BOOT_MATCH)));
  }
  
  @Test
  public void findByTitle() throws Exception {
    mockMvc.perform(get("/journal/findBy/title/" + CLOUD_MATCH))
      .andExpect(status().isOk())
      .andExpect(content().contentType(contentType));
//      .andExpect(jsonPath("$",iterableWithSize(1)))
//      .andExpect(jsonPath("$[0]['title']",containsString(CLOUD_MATCH)));
  }

  @Test
  public void add() throws Exception {
    mockMvc.perform(post("/journal")
		    .content(this.toJsonString(new JournalEntry("스트링 부트 테스트", "스프링 부트 단위 테스트를 생성했다","05/09/2016")))
		    .contentType(contentType)).andExpect(status().isOk());
  }
  
  @SuppressWarnings("unchecked")
  protected String toJsonString(Object obj) throws IOException {
    MockHttpOutputMessage mockHttpOutputMessage = new MockHttpOutputMessage();
    this.mappingJackson2HttpMessageConverter.write(obj, MediaType.APPLICATION_JSON, mockHttpOutputMessage);
    return mockHttpOutputMessage.getBodyAsString();
  }
}
```

- **WebApplicationContext** 
	- 웹 애플리케이션 구성 정보를 제공
    - MockMvc 인스턴스를 생성할 때 필요
    
- **HttpMessageConverter<T>**
	- HTTP 요청과 응답 간 메시지를 변환하는 도우미 인터페이스
	- 여기서는 JSON 형식의 테스트 문자열을 전송하는 데 쓰임
	- HTTP 메서드에 상관없이 잘 작동
- **MediaType.APPLICATION_JSON**
	- 오가는 콘텐트 타입이 JSON 형식의 테스트 문자열을 전송하는데 쓰임 

---

### Controller

```java
@RestController
public class JournalController {

  private static List<JournalEntry> entries = new ArrayList<JournalEntry>();
  static {
    try {
      entries.add(new JournalEntry("스프링 부트 입문", "오늘부터 스프링 부트를 배웠다", "01/01/2016"));
      entries.add(new JournalEntry("간단한 스프링 부트 프로젝트", "스프링 부트 프로젝트를 처음 만들어보았다", "01/02/2016"));
      entries.add(new JournalEntry("스프링 부트 해부", "스프링 부트를 자세히 살펴보았다", "02/01/2016"));
      entries.add(new JournalEntry("스프링 부트 클라우드", "클라우드 파운드리를 응용한 스프링 부트를 공부했다", "03/01/2016"));
    } catch (ParseException e) {
      e.printStackTrace();
    }
  }

  @RequestMapping("/journal/all")
  public List<JournalEntry> getAll() throws ParseException {
    return entries;
  }

  @RequestMapping("/journal/findBy/title/{title}")
  public List<JournalEntry> findByTitleContains(@PathVariable String title) throws ParseException {
    return entries
      .stream()
      .filter(entry -> entry.getTitle().toLowerCase().contains(title.toLowerCase()))
      .collect(Collectors.toList());
  }

  @RequestMapping(value="/journal",method = RequestMethod.POST )
  public JournalEntry add(@RequestBody JournalEntry entry) {
    entries.add(entry);
    return entry;
  }

}
```

### domain

```java
public class JournalEntry {

  private String title;
  private Date created;
  private String summary;

  private final SimpleDateFormat format = new SimpleDateFormat("MM/dd/yyyy");

  public JournalEntry(String title, String summary, String date) throws ParseException {
    this.title = title;
    this.summary = summary;
    this.created = format.parse(date);
  }

  JournalEntry() {}

  public String getTitle() {
    return title;
  }

  public void setTitle(String title) {
    this.title = title;
  }

  public Date getCreated() {
    return created;
  }

  public void setCreated(String date) throws ParseException {
    Long _date = null;
    try {
      _date = Long.parseLong(date);
      this.created = new Date(_date);
      return;
    } catch(Exception ex) {}
    this.created = format.parse(date);
  }

  public String getSummary() {
    return summary;
  }

  public void setSummary(String summary) {
    this.summary = summary;
  }

  public String toString() {
    StringBuilder value = new StringBuilder("* JournalEntry(");
    value.append("제목: ");
    value.append(title);
    value.append(", 요약: ");
    value.append(summary);
    value.append(", 작성일자: ");
    value.append(format.format(created));
    value.append(")");
    return value.toString();
  }
}
```

---

## Reference

[실전 스프링 부트 워크북](http://www.hanbit.co.kr/store/books/look.php?p_code=B2433442478)
