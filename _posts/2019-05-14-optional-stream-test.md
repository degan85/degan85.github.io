---
layout: post
title:  "Optional, Stream, json test"
author: Degan
categories: JAVA
tags:	stream 
comments: true
---

```java

import static org.assertj.core.api.Assertions.assertThat;

import java.io.IOException;
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.Optional;

import org.junit.Ignore;
import org.junit.Test;

import com.fasterxml.jackson.core.JsonParseException;
import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.core.type.TypeReference;
import com.fasterxml.jackson.databind.JsonMappingException;
import com.fasterxml.jackson.databind.ObjectMapper;

import abc.oyoung.www.model.Trend;
import abc.oyoung.www.model.Upload;

public class streamTest {
	
	public void streamTest() {
		
		Map map = new HashMap();
		Trend trend = new Trend();
		
//		map.put("tat", "tag");
//		map.put("content", "content");
//		map.put("hidden", false);
		
		String tag = (String) Optional.ofNullable(map.get("tag")).orElse("");
		String content = (String) Optional.ofNullable(map.get("content")).orElse("");
		boolean hidden = (boolean) Optional.ofNullable(map.get("boolean")).orElse(false);

		trend.setTag(tag);
		trend.setContent(content);
		trend.setHidden(hidden);
		
	}
	@Ignore
	@Test
	public void forEachTest() {
		
		Map map1 = new HashMap();
		map1.put("path", "path1");
		map1.put("path", null);
//		map1.put("realFileName", "realFileName1");

		Map map2 = new HashMap();
		map2.put("path", "path2");
		map2.put("realFileName", "realFileName2");
		
		List<Map> maptestList = new ArrayList<>();
		maptestList.add(map1);
		maptestList.add(map2);
		
		Map map = new HashMap();
		map.put("uploads", maptestList);
		
		

		List<Map> uploads = (List<Map>) Optional.ofNullable(map.get("uploads")).orElseGet(ArrayList::new);
		uploads.forEach(System.out::println);
		List<Upload> uploadList = new ArrayList();
		
		uploads.forEach(upload -> {
			String path = (String) Optional.ofNullable(upload.get("path")).orElse("");
			String fileName = (String) Optional.ofNullable(upload.get("realFileName")).orElse("");

            Upload tempUpload = new Upload();
            tempUpload.setPath(path+fileName);
            tempUpload.setFilename(fileName);
            uploadList.add(tempUpload);
		});
//		for(int i=0;i<uploads.size();i++) {
//			String path = (String) uploads.get(i).get("path");
//			String fileName = (String) uploads.get(i).get("realFileName");
//
//            Upload upload = new Upload();
//            upload.setPath(path+fileName);
//            upload.setFilename(fileName);
//            uploadList.add(upload);
//		}
		
		uploadList.forEach(upload -> {
			System.out.println(upload.getPath());
			System.out.println(upload.getFilename());
		});
	}
	
	@Ignore
	@Test
	public void jsonTest() throws JsonProcessingException {
		
		HashMap<String, Object> values = new HashMap<>();
		values.put("category", "category");
		values.put("name", "name");
		values.put("sex", "sex");
		
		String json = new ObjectMapper().writeValueAsString(values);
		System.out.println(json);
	}

	@Test
	public void filterTest() throws JsonParseException, JsonMappingException, IOException {
		
		// when
		String strArray = "[{\"consultantName\":\"APGUJEONG FEEL Plastic Surgery\",\"consultantId\":\"10015\",\"quotation\":[{\"category\":\"Nose\",\"price\":\"2342000\"},{\"category\":\"Breast\",\"price\":\"1000000\"}]},{\"consultantName\":\"APGUJEONG FEEL Plastic Surgery\",\"consultantId\":\"10014\",\"quotation\":[{\"category\":\"Face\",\"price\":\"2342000\"},{\"category\":\"Body\",\"price\":\"1000000\"}]}]";
		List<HashMap<String,Object>> json = new ObjectMapper().readValue(strArray, new TypeReference<List<Map<String,Object>>>() {
		});
	
		// then
		assertThat(json.size()).isEqualTo(2);
		
		// when
		json.forEach(j -> {
			if("10015".equals(j.get("consultantId")) && j.get("quotation").toString().contains("Nose")) {
				j.put("selectedDate","test");
			}
//			System.out.println();
		});
		
		// then
		json.forEach(System.out::println);
		
		// when
		
	}
}

```

