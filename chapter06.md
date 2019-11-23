# 객체지향 프로그래밍

## 8.2.2 $("#myDiv").text() 살펴보기
- $("#myDiv") 실행결과로 생성된 jQuery 객체 인스턴스에는 text()메서드가 없다
  text() 메서드는 jQuery의 표준 API이므로 jQuery.prototype 객체에 있다
```js
text: function(e) {
		e = e || this;
		var t = "";
		for ( var j = 0; j < e.length; j++ ) {
			var r = e[j].childNodes;
			for ( var i = 0; i < r.length; i++ )
				t += r[i].nodeType != 1 ?
					r[i].nodeValue : jQuery.fn.text([ r[i] ]);
		}
		return t;
	},
```
![8-13](https://user-images.githubusercontent.com/53419312/69480075-7bc20080-0e47-11ea-8802-f887a2b01695.PNG)

### DOM 객체 타입과 값
- 모든 객체는 자신이 어떤 타입인지를 나타내는 nodeType이라는 프로퍼티가 있다
- Node.ELEMNET_NODE==1 (태그)
- Node.ELEMNET_NODE==2 (속성)
- Node.ELEMNET_NODE==3 (문자열)
- Node.ELEMNET_NODE==8 (주석)
- Node.ELEMNET_NODE==0 (Document)
- 각 DOM 노드 중 일부 타입 객체의 경우는 nodeValue 프로퍼티로 자신이 가지고 있는 문자열 값을 제공한다

## 8.3.1 jQuey 이벤트 처리 예제
```
<!DOCTYPE html>
<html>
<head>
    <!--jQuery 소스 경로는 자신에 맞게 -->
    <script src="jquery.js"></script>
</head>
<body>
<div id="clickDiv">Click Here</div>
<script>
    $('#clickDiv').click(function(){
        alert('Mouse Click');
    });
</script>
</body>
</html>
```

## 8.3.2 click() 메서드 정의
```js
new function(){

	var e = ("blur,focus,load,resize,scroll,unload,click,dblclick," +
		"mousedown,mouseup,mousemove,mouseover,mouseout,change,reset,select," + 
		"submit,keydown,keypress,keyup,error").split(",");

	for ( var i = 0; i < e.length; i++ ) new function(){
			
		var o = e[i];
		
		jQuery.fn[o] = function(f){
			return f ? this.bind(o, f) : this.trigger(o);
		};
	
	};
			
};
```

## 8.3.3 $('#clickDiv').click() 호출 코드 분석
```js
$('#clickDiv').click(function(){
    alert('Mouse Click');
});
```
![8-17](https://user-images.githubusercontent.com/53419312/69480093-a1e7a080-0e47-11ea-8724-c3469bc1b060.PNG)

인자 f로는 click() 메서드를 호출 할 때 인자로 넘긴 함수 표현식 function(){alert('Mouse Click');}이 그대로 전달 