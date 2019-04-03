---
layout: post
title:  "Html5 canvas thumbnail"
author: Degan
categories: Html5
tags:   html5 canvas thumbnail	
comments: true
---

### html
```html

    <img id="preview" src="" width="400" alt="local img"/>
    <a id="download" download="thumbnail.jpg" target="_blank">
        <img id="thumbnail" src="" width="80" alt="thumbnail img">
    </a>
    <input type="file" id="getfile" accept="image/*">
```

### javascript
```javascript

var file = document.querySelector('#getfile');

file.onchange = function() {
    var fileList = file.files;

    //read
    var reader = new FileReader();
    reader.readAsDataURL(fileList[0]);

    //after load
    reader.onload = function() {
        document.querySelector('#preview').src = reader.result;

        //thumbnail img
        var tempImage = new Image(); // drawImage 메서드에 넣기 위해 이미지 객체화
        tempImage.src = reader.result; // data-uri를 이미지 객체에 주입
        tempImage.onload = function() {

            // 리사이즈를 위해 캔버스 객체 생성
            var canvas = document.createElement('canvas');
            var canvasContext = canvas.getContext("2d");

            // 캔버스 크기 설정
            canvas.width = 100;
            canvas.height = 100;

            // 이미지를 캔버스에 그리기
            canvasContext.drawImage(this, 0, 0, 100, 100);

            // 캔버스에 그린 이미지를 다시 data-uri 형태로 변환
            var dataURI = canvas.toDataURL("image/jpeg");

            // 썸네일 이미지 보여주기
            document.querySelector('#thumbnail').src = dataURI;

            // 썸네일 이미지를 다운로드 할 수 있도록 링크 설정
            document.querySelector('#download').href = dataURI;

        }
    }
}
```
---

## Reference

[HTML5 File API 기초부터 썸네일 이미지 생성까지](https://programmingsummaries.tistory.com/367)
