---
layout: post
title: "[토비의 스프링 3.1] 3. 템플릿"
author: Degan
categories: spring
tags:	spring object toby Ioc DI template callback pattern
comments: true
---

### 예외처리
*  예외가 발생하면 메소드실행 중단
*  일반적으로 서버에서는 **제한된 개수의 DB 커넥션을 만들어 재사용 가능한 풀로 관리**
*  DB 풀은 매번 `getConnection()`으로 가져간 커넥션을 명시적으로 `close()`해서 돌려줘야함
*  풀에 있는 리소스가 고갈되지 않게 빠르게 반환해야 함

```java
public void deleteAll() trows SQLException {
	Connection c = dataSource.getConnection();

	PreparedStatement ps = c.prepareStatement("delete from users");
	ps.executeUpdate();

	ps.close();
	c.close();
```

- 예외 처리
```java
public void deleteAll() trows SQLException {
		
		Connection c = null;
		PreparedStatement ps = null;
		
		try {
			c = dataSource.getConnection();
			ps = c.prepareStatement("delete from users");
			ps.executeUpdate();
			
		} catch (SQLException e) {
			throw e;
		} finally {
			if(ps != null) { try { ps.close(); } catch(SQLException e) {} }
			if(c != null) { try { c.close(); } catch(SQLException e) {} }
		}
	}
```
- `finally`안에도 `try-catch`해야 함

### 템플릿 메소드 패턴
 * 상속을 통해 기능을 확장
 * 변하지 않는 부분은 슈퍼 클래스로 두고 변하는 부분은 추상 메소드로 정의
 * 서브클래스에서 오버라이드하여 새롭게 정의해서 사용
 
```java
public class UserDaoDeleteAll extends UserDao2{
	
	protected PreparedStatement makeStatement(Connection c) throws SQLException {
		PreparedStatement ps = c.prepareStatement("delete from users");
		return ps;
	}
}
```

* 단점 : 
	 * DAO 로직마다 새로운 서브클래스를 생성해야 함
		 * ex) delete, add, update ...
	 * 컴파일 시점에 이미 관계가 결정되어 유연성이 떨어짐

### 전략 패턴
 * 개방 폐쇄 원칙(OCP)을 잘 지키는 구조이면서 
 * 템플릿 메소드보다 유연하고 확장성이 뛰어남 
 * 오브젝트를 아애 둘로 분리하고 클래스 레벨에서는 인터페이스를 통해서만 의존
 
  1. DB 커넥션 가져오기
  2. PreparedStatement를 만들어줄 외부 기능 호출하기 => 전략(변하는 부분)
  3. 전달받은 PreparedStatement 실행하기
  4. 예외가 발생하면 이를 다시 메소드 밖으로 던지기
  5. 모든 경우에 만들어진 PreparedStatement와 Connection을 적절히 닫아주기

```java
public interface StatementStrategy {
	PreparedStatement makePreparedStatement(Connection c) throws SQLException;
}
```

```java
public class DeleteAllStatement implements StatementStrategy {

	@Override
	public PreparedStatement makePreparedStatement(Connection c) throws SQLException {
		PreparedStatement ps = c.prepareStatement("delete from users");
		return ps;
	}
	
}
```

```java
public void deleteAll() throws SQLException {
		...
		try {
			c = dataSource.getConnection();
			
			StatementStrategy strategy = new DeleteAllStatement();
			ps = strategy.makePreparedStatement(c);
			
			ps.executeUpdate();
		}catch (SQLException e) {
		...
```

- 개선해야 하는 이유
	- **전략 패턴**은 필요에 따라 컨텍스트는 그대로 유지되면서(OCP의 폐쇄 원칙)
	- 전략을 바꿔 쓸 수 있는 것인데(OPC의 개방 원칙)
	- 컨텍스트 안에서 이미 구체적인 전략 클래스(`DeleteAllStatement`)를 
	- 사용하도록 **고정**되어 있다면 **이상함**

### DI 적용을 위한 클라이언트/컨텍스트 분리
- 전략 오브젝트 생성과 
- 컨텍스트로의 전달을 담당하는 책임을 분리(ObjectFactory)
- 이를 일반화한 것이 의존관계 주입(DI)
- 결국 전략 패턴의 장점을 일반적으로 활용할 수 있도록 만든 구조

```java
	public void deleteAll() throws SQLException {
		StatementStrategy strategy = new DeleteAllStatement();
		jdbcContextWithStatementStrategy(st);
	}
```

```java
	public void jdbcContextWithStatementStrategy(StatementStrategy stmt) throws SQLException {
		Connection c = null;
		PreparedStatement ps = null;

		try {
			c = this.dataSource.getConnection();
			ps = stmt.makePreparedStatement(c);
			ps.executeUpdate();
			
		} catch (SQLException e) {
			e.printStackTrace();
		} finally {
			if(ps != null) { try { ps.close(); } catch(SQLException e) {} }
			if(c != null) { try { c.close(); } catch(SQLException e) {} }
		}
	}
```

- 개선
	- DAO 메소드마다 새로운 StatementStrategy 구현 클래스를 만들어야 함
		- ex) DeleteAllStatement, AddStatement ... 
		- 클래스 파일 개수가 많이 늘어남
	- DAO 메소드에서 StatementStrategy에 전달할 매개변수가 있는 경우
	- 이를 위해 오브젝트를 전달받는 생성자와
	- 이를 저장해둘 인스턴스 변수를 번거롭게 만들어야 함
		- ex) UserDao 등
	- **로컬 클래스로 위의 단점들을 개선**
	- **익명 내부 클래스로 더 개선**
		- new 인터페이스이름() { 클래스 본문 };

---

#### 전략 패턴의 기본 구조에 익명 내부 클래스를 활용한 방식
#### 이런 방식을 스프링에서는 템플릿/콜백 패턴이라고 부름
#### 전략 패턴의 컨텍스트를 템플릿
#### 익명 내부 클래스로 만들어진 오브젝트를 콜백

![템플릿/콜백](http://cfile29.uf.tistory.com/image/122C37544D3535FB1FE5CF)

 ---
 
### 템플릿/콜백 예제
- `Calculator` 계산 클래스를 예로 구현 
- `calSum`라는 txt파일에 있는 숫자들을 더하는 메소드 생성
- IO 예외를 처리해줘야 함
- **고객이 더하기 말고 곱하기도 요청한다면?**
 
```java

public class Calculator {

	public Integer calSum(String filepath) throws IOException {
		BufferedReader br = null;
		
		try {
			br = new BufferedReader(new FileReader(filepath));
			Integer sum = 0;
			String line = null;
			while((line = br.readLine()) != null) {
				sum += Integer.valueOf(line);
			}
			return sum;
		} catch (IOException e) {
			e.printStackTrace();
			throw e;
		} finally {
			if(br != null) { // BufferReader 오브젝트가 생성되기 전에 예외가 발생할 수도 있음
				try{ br.close(); } 
				catch(IOException e) {System.out.println(e.getMessage()); }
			}
		}
	}
```

### 더하기 말고 숫자를 이용한 기능들 추가 요청
* 템플릿/콜백 패턴 적용
* 템플릿과 콜백의 경계를 정하고 
* 템플릿이 콜백에게, 콜백이 템플릿에게 각각 전달하려는 내용이 무엇인지 파악
* 그에 따라 인터페이스를 정의해야 함

- 인터페이스
```java
public interface BufferedReaderCallBack {
	Integer doSomethingWithReader(BufferedReader rd) throws IOException;
}
```

- 반복되는 템플릿
```java
private Integer fileReadTemplate(String filepath, BufferedReaderCallBack callback) throws IOException {
		BufferedReader br = null;
		try {
			br = new BufferedReader(new FileReader(filepath));
			int ret = callback.doSomethingWithReader(br);	// 콜백 오브젝트 호출
			return ret;
		} catch (IOException e) {
			e.printStackTrace();
			throw e;
		} finally {
			if(br != null) {
				try{ br.close(); } 
				catch(IOException e) {System.out.println(e.getMessage()); }
			}
		}
	}
```

- 수정된 `calSum` 메소드

```java
public Integer calSum(String filepath) throws IOException {
		
		BufferedReaderCallBack sumCallBack =  new BufferedReaderCallBack() {
			public Integer doSomethingWithReader(BufferedReader rd) throws NumberFormatException, IOException {
				Integer sum = 0;
				String line = null;
				while((line = rd.readLine()) != null) {
					sum += Integer.valueOf(line);
				}
				return sum;
			}
		};
		return fileReadTemplate(filepath, sumCallBack);
	}
```

### 스프링의 JdbcTemplate
- 스프링은 JDBC를 이용하는 DAO에서 사용할 수 있도록
- 다양한 템플릿과 콜백을 제공
- 예제어서 만들었던 `JdbcContext`와 유사하지만 훨씬 강력하고 편리함
- `JdbcContext`를 버리고 `JdbcTemplate`로 변경
- `JdbcTemplate`을 생성하면 `DataSource`를 직접 DI해줌

```java
public class UserDao2 {

	private JdbcTemplate jdbcTemplate;
	
	public void setDataSource(DataSource dataSource) {
		this.jdbcTemplate = new JdbcTemplate(dataSource);
	}
```

#### update()
* `StatementStrategy` 인터페이스의 `makePreparedStatement()` 콜백에 대응
* `JdbcTemplate`의 콜백은 
* `PreparedStatementCreator` 인터페이스의 `createPreparedStatement()` 메소드
* `Connection`을 제공받아서 `PreparedStatement`를 만들어 돌려 준다는 면에서 구조가 동일

```java
	public void deleteAll() throws SQLException, ClassNotFoundException {
			this.jdbcTemplate.update( new PreparedStatementCreator() {
				public PreparedStatement createPreparedStatement(Connection con) throws SQLException {
					return con.prepareStatement("delete from users");
				}
			});
		}
```
 
 - 또는 `JdbcTemplate`의 내장 콜백을 사용하는 메소드 호출로 수정

```java
	public void deleteAll() throws SQLException, ClassNotFoundException {
		this.jdbcTemplate.update("delete from users");
	}
```

#### queryForInt()
* PreparedStatementCreator 콜백과
* ResultSetExtractor 콜백을 파라미터로 받는 query() 메소드
* 첫 번째 PreparedStatementCreator 콜백은 템플릿으로부터 Connection을 받고
* PreparedStatement를 돌려줌
* 두 번째 ResultSetExtractor는 템플릿으로부터 ResultSet을 받고 거기서 추출한 결과를 돌려줌 

```java
public int getCount() throws SQLException, ClassNotFoundException {
		
		return this.jdbcTemplate.query(new PreparedStatementCreator() {
			public PreparedStatement createPreparedStatement(Connection con) throws SQLException {
				return con.prepareStatement("select count(*) from users");
			}
		}, new ResultSetExtractor<Integer>() {
			public Integer extractData(ResultSet rs) throws SQLException, DataAccessException {
				rs.next();
				return rs.getInt(1);
			}
		});
	}
```  

- 또는 다음과 같이 호출(**queryForInt**)

```java
	public int getCount() {
		return this.jdbcTemplate.queryForInt("select * from users");
	}
```

#### queryForObject()
* ResultSet의 결과를 User 오브젝트를 만들어 프로퍼티에 넣어줘야 함
* ResultSetExtractor 콜백 대신 RowMapper 콜백 사용
* ResultSet을 전달받고, 필요한 정보를 추출해서 리턴하는 방식
* ResultSet의 로우 하나를 매핑하기 위해 사용되기 때문에 여러 번 호출될 수 있음

```java
public User get(String id) throws ClassNotFoundException, SQLException {
	
	return this.jdbcTemplate.queryForObject("select * from users where id =?", new Object[] {id},
			new RowMapper<User>() { public User mapRow(ResultSet rs, int rowNum) throws SQLException {
				User user = new User();
				user.setId(rs.getString("id"));
				user.setName(rs.getString("name"));
				user.setPassword(rs.getString("password"));
				return user;
				}
	});
}
```

### 스프링에는 다양한 템플릿/콜백 패턴을 적용한 API가 존재함
### 클래스 이름이 Template으로 끝나거나 인터페이스 이름이 Callback으로 끝나는 것

## Reference

[토비의 스프링 3.1](http://www.acornpub.co.kr/book/toby-spring3.1-vol1)
