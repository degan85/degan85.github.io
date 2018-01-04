---
layout: post
title:  "[Head First Design Patterns] Observer Pattern"
author: Degan
categories: Design_Pattern
tags:	java design pattern 
comments: true
---

## Observer Pattern
- 한 객체의 상태가 바뀌면 그 객체에 의존하는 다른 객체들한테 연락이 가고 자동으로 내용이 갱신되는 방식
- 일대다(one-to-many) 의존성을 정의 함
- 대부분 **Subject 인터페이스**와 **Observer 인터페이스**가 있는 클래스로 디자인
- **Subject** 또는 **Observable** 객체는 동일한 인터페이스를 써서 **Observer**에게 연락을 함
	- push 방식 :  Subject 객체에서 데이터를 보내는 방식
	- pull 방식 : 옵저버가 데이터를 가져오는 방식

## 클래스 다이어그램
![observer pattern](https://pic002.cnblogs.com/images/2012/358984/2012042511105376.png)
-[출처: Head First Design Patterns - Observer Pattern](http://www.cnblogs.com/zhuqiang/archive/2012/04/25/2469515.html)

## 기상 스테이션 sample
- WeatherData 객체는 기상 스테이션 장비로부터 데이터를 받음
- 온도, 습도, 압력이 변하면 디스플레이 장비에 갱신

### 직접 구현
- subject interface

```java
public interface Subject {
	public void registerObserver(Observer o);
	public void removeObserver(Observer o);
	public void notifyObservers();
}
```

- observer interface

```java
public interface Observer {
	public void update(float temp, float humidity, float pressure);
}
```

- DisplayElement interface

```java
public interface DisplayElement {
	public void display();
}
```

---

- WeatherData
- subject interface 구현
- 다음 예제는 push 방식

```java
public class WeatherData implements Subject{
	
	private ArrayList<Observer> observers;
	private float temperature;
	private float humidity;
	private float pressure;
	
	public WeatherData() {
		observers = new ArrayList<Observer>();
	}
	
	public void registerObserver(Observer o) {
		observers.add(o);
	}
	
	public void removeObserver(Observer o) {
		int i = observers.indexOf(o);
		if(i >= 0) {
			observers.remove(i);
		}
	}
	
	public void notifyObservers() {
		for(int i=0; i<observers.size(); i++) {
			Observer observer = (Observer) observers.get(i);
			observer.update(temperature, humidity, pressure);
		}
	}
	
	public void setMeasurementsChange(float temperature, float humidity, float pressure) {
		this.temperature = temperature;
		this.humidity = humidity;
		this.pressure = pressure;
		measurementsChange();
	}

	private void measurementsChange() {
		notifyObservers();
	}
}
```

- `Observer`객체들을 저장하기 위해 `ArrayList`사용
- `notifyObserver()`에서 상태에 대해 모든 observer들에게 알림
- **observer**들은 **Observer interface**를 구현해서 모두 `update()`메소드가 있음

---

- CurrentConditionsDisplay
- Observer interface, DisplayElement interface 구현

```java
public class CurrentConditionsDisplay implements Observer, DisplayElement{
	private float temperature;
	private float humidity;
	private Subject weatherData;
	
	public CurrentConditionsDisplay(Subject weatherData) {
		this.weatherData = weatherData;
		weatherData.registerObserver(this);
	}
	
	public void update(float temperature, float humidity, float pressure) {
		this.temperature = temperature;
		this.humidity = humidity;
		display();		
	}
	
	public void display() {
		System.out.println("Current conditions: "+temperature+"C degrees and "+humidity+"% humidity");
	}
}
```

- `WeatherData` 객체로부터 변경 사항을 받기 위해 `Observer interface` 구현
- 생성자에 subject(WeatherData) 전달
- 해당 디스플레이를 옵저버로 등록
- `update()`가 호출되면 기온과 습도를 저장하고 `display()` 호출

---

### 자바 API 이용
- java.util 패키지에
	- Observer interface
	- Observable class
- subject 대신 Observable 사용
- interface가 아니라서 `WeatherData`에서 **확장**(상속)을 해야 함
- 언제 Observer한테 연락할지만 알려주면 **API가 알아서 처리**

---

- WeatherData
- 다음 예제는 push 방식

```java
public class WeatherData extends Observable{
	private float temperature;
	private float humidity;
	private float pressure;
	
	public WeatherData() { }
	
	public void setMeasurementsChange(float temperature, float humidity, float pressure) {
		this.temperature = temperature;
		this.humidity = humidity;
		this.pressure = pressure;
		measurementsChanged();
	}

	private void measurementsChanged() {
		setChanged();
		notifyObservers();
	}

	public float getTemperature() {
		return temperature;
	}

	public float getHumidity() {
		return humidity;
	}

	public float getPressure() {
		return pressure;
	}
}
```

- `setChanged()`를 먼저 호출해서 객체의 상태가 변한 것을 알림
	- observer을 갱신하는 방법을 더 유연하게 하기 위해
- 생성자에서 Observer들을 저장하기 위한 자료 구조를 만들 필요가 없음
	- 슈퍼클래스에서 등록, 탈퇴, 연락을 다 진행
- `notifyObservers()`를 호출할 때 데이터 객체를 보내지 않음
	- **pull 모델**을 사용하고 있음
	- Observer가 getter를 이용해서 데이터를 가져옴

---

- CurrentConditionsDisplay

```java
public class CurrentConditionsDisplay implements Observer, DisplayElement{
	Observable observable;
	private float temperature;
	private float humidity;
	
	public CurrentConditionsDisplay(Observable observable) {
		this.observable = observable;
		observable.addObserver(this);
	}
	
	@Override
	public void update(Observable obs, Object arg) {
		if(obs instanceof WeatherDataUsedObservable) {
			WeatherDataUsedObservable weatherData = (WeatherDataUsedObservable) obs;
			this.temperature = weatherData.getTemperature();
			this.humidity = weatherData.getHumidity();
			display();
		}
	}

	@Override
	public void display() {
		System.out.println("Current conditions: "+temperature+"C degrees and "+humidity+"% humidity");
	}
	
}
```

- **java.util 패키지에 Observer interface** 구현
- 생성자에서 **Observable 레퍼런스를 사용해** Observer 등록 
- `update()`메소드에서 기온과 습도를 가져와 `display()` 호출

---

- 테스트

```java
public class TestObserverPattern {
	private WeatherData weatherData;
	
	@Before
	public void setUp() {
		weatherData = new WeatherData();
	}
	
	@Test
	public void test() {
		CurrentConditionsDisplay currentDisplay = new CurrentConditionsDisplay(weatherData);
		StatisticsDisplay staticDisplay = new StatisticsDisplay(weatherData);
		
		weatherData.setMeasurementsChange(80, 65, 30.4f);
		assertTrue(true);
	}
	
}
```

---

## Reference

[Head First Design Patterns](http://www.hanbit.co.kr/store/books/look.php?p_code=B9860513241)
