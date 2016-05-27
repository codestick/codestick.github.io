---
layout: post
title: Test markdown
subtitle: Each post also has a subtitle
---

You can write regular [markdown](http://markdowntutorial.com/) here and Jekyll will automatically convert it to a nice webpage.  I strongly encourage you to [take 5 minutes to learn how to write in markdown](http://markdowntutorial.com/) - it'll teach you how to transform regular text into bold/italics/headings/tables/etc.

**Here is some bold text**

## Here is a secondary heading


And here is some code with syntax highlighting

```java
@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private Long id;
 
    private String firstName;
    private String lastName;
    private String email;
 
    private int age;
}
```

```java
package com.fvthree.exception;

import org.springframework.http.HttpStatus;
import org.springframework.web.bind.annotation.ResponseStatus;

@ResponseStatus(HttpStatus.NOT_FOUND)
public class ResourceNotFoundException extends RuntimeException {
	
	private static final long serialVersionUID = 1L;
	
	public ResourceNotFoundException() {}
	
	public ResourceNotFoundException(String message) {
		super(message);
	}
	
	public ResourceNotFoundException(String message, Throwable cause) {
		super(message, cause);
	}
}

```

```java
package edu.zipcloud.cloudstreetmarket.core.dtos;

import java.math.BigDecimal;

public class MarketOverviewDTO {

	private String marketShortName;
	private String marketId;
	private BigDecimal latestValue;
	private BigDecimal latestChange;
	
	public MarketOverviewDTO(String marketShortName, String marketId, BigDecimal latestValue, BigDecimal latestChange) {
		
		this.marketShortName = marketShortName;
		this.marketId = marketId;
		this.latestValue = latestValue;
		this.latestChange = latestChange;
	}
	
	public String getMarketShortName() {
		return marketShortName;
	}
	
	public void setMarketShortName(String marketShortName) {
		this.marketShortName = marketShortName;
	}
	
	public String getMarketId() {
		return marketId;
	}
	
	public void setMarketId(String marketId) {
		this.marketId = marketId;
	}
	
	public BigDecimal getLatestValue() {
		return latestValue;
	}
	
	public void setLatestValue(BigDecimal latestValue) {
		this.latestValue = latestValue;
	}
	
	public BigDecimal getLatestChange() {
		return latestChange;
	}
	
	public void setLatestChange(BigDecimal latestChange) {
		this.latestChange = latestChange;
	}
	
}
```

```java
@Override
  public void applyCSSPropertyBackgroundColor(Object element, CSSValue value,
      String pseudo, CSSEngine engine) throws Exception {
    Widget widget = (Widget) ((WidgetElement) element).getNativeWidget();
    if (value.getCssValueType() == CSSValue.CSS_PRIMITIVE_VALUE) {
      Color newColor = (Color) engine.convert(value, Color.class, widget
          .getDisplay());
      if (widget instanceof CTabItem) {
        CTabFolder folder = ((CTabItem) widget).getParent();
        if ("selected".equals(pseudo)) {
          // tab folder selection manages gradients
          CSSSWTColorHelper.setSelectionBackground(folder, newColor);
        } else {
          CSSSWTColorHelper.setBackground(folder, newColor);
        }
      } else if (widget instanceof Control) {
        GradientBackgroundListener.remove((Control) widget);
        CSSSWTColorHelper.setBackground((Control) widget, newColor);
        CompositeElement.setBackgroundOverriddenByCSSMarker(widget);
      }
    } else if (value.getCssValueType() == CSSValue.CSS_VALUE_LIST) {
      Gradient grad = (Gradient) engine.convert(value, Gradient.class,
          widget.getDisplay());
      if (grad == null) {
        return; // warn?
      }
      if (widget instanceof CTabItem) {
        CTabFolder folder = ((CTabItem) widget).getParent();
        Color[] colors = CSSSWTColorHelper.getSWTColors(grad,
            folder.getDisplay(), engine);
        int[] percents = CSSSWTColorHelper.getPercents(grad);

        if ("selected".equals(pseudo)) {
          folder.setSelectionBackground(colors, percents, true);
        } else {
          folder.setBackground(colors, percents, true);
        }

      } else if (widget instanceof Control) {
        GradientBackgroundListener.handle((Control) widget, grad);
        CompositeElement.setBackgroundOverriddenByCSSMarker(widget);
      }
    }
  } 
```

```java
package com.fvthree.client;

import java.net.URI;
import java.util.HashSet;
import java.util.Set;

import org.springframework.core.ParameterizedTypeReference;
import org.springframework.http.HttpMethod;
import org.springframework.http.ResponseEntity;
import org.springframework.web.client.RestTemplate;
import org.springframework.web.util.UriComponentsBuilder;

import com.fvthree.domain.Option;
import com.fvthree.domain.Poll;

public class QuickPollClientV2 {
	private static final String QUICK_POLL_URI_2 = "http://localhost:8080/v2/polls";
	private RestTemplate restTemplate = new RestTemplate();

	public PageWrapper<Poll> getAllPolls(int page, int size) {
		ParameterizedTypeReference<PageWrapper<Poll>> responseType = new ParameterizedTypeReference<PageWrapper<Poll>>() {};
		UriComponentsBuilder builder = UriComponentsBuilder
				.fromHttpUrl(QUICK_POLL_URI_2)
				.queryParam("page", page)
				.queryParam("size", size);
		ResponseEntity<PageWrapper<Poll>> responseEntity = restTemplate
				.exchange(builder.build().toUri(), HttpMethod.GET, null, responseType);
		return responseEntity.getBody();
	}
	
	public Poll getPollById(Long pollId) {
		return restTemplate.getForObject(QUICK_POLL_URI_2 + "/{pollId}", Poll.class, pollId);
	}
	
	public URI createPoll(Poll poll) {
		return restTemplate.postForLocation( QUICK_POLL_URI_2, poll);
	}
	
	public void updatePoll(Poll poll) {
		restTemplate.put(QUICK_POLL_URI_2 + "/{pollId}",  poll, poll.getId()); 
	}
	
	public void deletePoll(Long pollId) {
		restTemplate.delete(QUICK_POLL_URI_2 + "/{pollId}",  pollId);
	}
	
	public static void main(String[] args) {
		QuickPollClientV2 client = new QuickPollClientV2();
		
		// Test GetPoll
		Poll poll = client.getPollById(1L);
		System.out.println(poll);
		
		// Test getAllPolls
		PageWrapper<Poll> allPolls = client.getAllPolls(2, 3);
		System.out.println(allPolls);
		
		// Test Create Poll
		Poll newPoll = new Poll();
		newPoll.setQuestion("What is your favourate color 2?");
		Set<Option> options = new HashSet<>();
		newPoll.setOptions(options);
		
		Option option1 = new Option();
		option1.setValue("Red");
		options.add(option1);
		
		Option option2 = new Option();
		option2.setValue("Blue");
		options.add(option2);
		URI pollLocation = client.createPoll(newPoll);
		System.out.println("Newly Created Poll Location " + pollLocation);
		
		// Test Update Poll with Id 6
		Poll pollForId6 = client.getPollById(6L);
		// Add a new Option
		Option newOption = new Option();
		newOption.setValue("The Incredibles 2");
		pollForId6.getOptions().add(newOption);
		
		client.updatePoll(pollForId6);
		
		pollForId6 = client.getPollById(6L);
		System.out.println("Updated poll has " + pollForId6.getOptions().size() + " options");
		
		// Test Delete
		client.deletePoll(9L);
	}
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>

	<groupId>com.fvthree</groupId>
	<artifactId>quick-poll</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<packaging>jar</packaging>

	<name>quick-poll</name>
	<description>quick poll application built with spring boot</description>

	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>1.3.5.RELEASE</version>
		<relativePath /> <!-- lookup parent from repository -->
	</parent>

	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<java.version>1.8</java.version>
	</properties>

	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-data-jpa</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>

		<dependency>
			<groupId>org.hsqldb</groupId>
			<artifactId>hsqldb</artifactId>
			<scope>runtime</scope>
		</dependency>

		<!-- http://mvnrepository.com/artifact/javax.inject/javax.inject -->
		<dependency>
			<groupId>javax.inject</groupId>
			<artifactId>javax.inject</artifactId>
			<version>1</version>
		</dependency>

		<dependency>
			<groupId>com.mangofactory</groupId>
			<artifactId>swagger-springmvc</artifactId>
			<version>1.0.2</version>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-security</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.security.oauth</groupId>
			<artifactId>spring-security-oauth2</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-web</artifactId>
		</dependency>

		<!-- http://mvnrepository.com/artifact/commons-io/commons-io -->
		<dependency>
			<groupId>commons-io</groupId>
			<artifactId>commons-io</artifactId>
			<version>2.5</version>
		</dependency>

		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-test</artifactId>
			<scope>test</scope>
		</dependency>

		<dependency>
			<groupId>com.jayway.jsonpath</groupId>
			<artifactId>json-path-assert</artifactId>
			<version>2.0.0</version>
		</dependency>

		<dependency>
			<groupId>org.springframework.hateoas</groupId>
			<artifactId>spring-hateoas</artifactId>
			<version>0.17.0.RELEASE</version>
		</dependency>

	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>


</project>
```
