# 자바스크립트 데이터 타입과 연산자

## 3.5배열
- 크기를 지정하지 않아도 된다
- 어떤위치에 어느 타입의 데이터를 저장하더라도 에러가 발생하지 않는다

## 3.5.1 배열 리터럴
- 객체 리터럴 : 중괄호 사용, 프로퍼티 이름과 프로퍼티값을 모두 표기,  표기법을 사용하여 해당 프로퍼티에 접근
- 배열 리터럴 : 대괄호 사용, 각 요소의 값만 포함, 배열 내 위치 인덱스값을 넣어서 접근
```
var colorArr = ['orange', 'yellow', 'blue', 'green'];
console.log(colorArr[0]); //orange
console.log(colorArr[1]); //yellow
console.log(colorArr[3]); //green
```

## 3.5.2 배열의 요소 생성
- 배열도 동적으로 배열 원소를 추가 할 수 있다
```
//빈 배열
var emptyArr= [];
console.log(emptyArr[0]); //undefined

//배열 요소 동적 생성
emptyArr[0] = 100;
emptyArr[3] = 'eight';
emptyArr[7] = true;
console.log(emptyArr); //[100, undefined, undefined, "eight:, undefined, undefined, undefined, true]
consoloe.log(emptyArr,length); //8
```

## 3.5.3 배열의 length 프로퍼티
length 프로퍼티는 배열 내에서 가장 큰 인덱스에 1을 더한 값이다
```
var arr= [];
console.log(arr.length);
arr[0] = 0; //arr.length = 1
arr[1] = 1; //arr.length = 2
arr[2] = 2; //arr.length = 3
arr[100] = 100; 
console.log(arr.length); //101
```
```
var arr = [0, 1, 2];
console.log(arr.length); //3

arr.length = 5;
console/log(arr); //[0, 1, 2, undefined, undefined]

arr.length = 2;
console.log(arr); //[0, 1]
console.log(arr[2]); //undefined
```
## 3.5.3.1 배열 표준 메서드와 length 프로퍼티
push() : 배열의 끝에 추가하는 자바스크립트 표준 배열 메서드 (배열의 끝에 추가 = length값의 위치에 추가)
```
var arr=['zero', 'one', 'two'];

//push메서드 호출
arr.push('three');
console.log(arr); //['zero', 'one', 'two', 'three']

//length 값 변경 후 push()메서드 호출
arr.length = 5;
arr.push('four');
console.log(arr); //['zero', 'one', 'two', 'three', undefined, 'four']
```

## 3.5.4 배열과 객체
```
//colorArray 배열
var colorArray = ['orange', 'yellow', 'green'];
console.log(colorArray[0]); //orange
console.log(colorArray[1]); //yellow
console.log(colorArray[2]); //green

//colorsObj 객체
var consoleObj = {
    '0' : 'orange',
    '1' : 'yellow',
    '2' : 'green'
};
console.log(colorObj[0]); //orange
console.log(colorObj[1]); //yellow
console.log(colorObj[2]); //green

//typeof 연산자 비교
console.log(typeof colorsArray); //object(not array)
console.log(typeof colorsObj); //object

//length 프로퍼티
console.log(colorsArray.length); //3
console.log(colorsObj.length); //undefined

//배열 표준 메서드
console.log(colorsArray.push); // ['orange', 'yellow', 'green']
console.log(colorsObj.push); //no method
```
