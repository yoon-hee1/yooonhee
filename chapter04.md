# 자바스크립트 데이터 타입과 연산자

## 4.4.3 함수 리턴
- 자바스크립트는 항상 함수값을 리턴한다

1) 일반 함수나 메서드는 리턴값을 지정하지 않을 경우, ,undefined 값이 리턴된다
```js
//noReturnFunc() 함수
var noReturnFunc = function(){
    console.log('This function has no return statement.'); //This function has no return statement.
};

var result = noReturnFunc();
console.log(result); //undefined
```
2) 생성자 함수에서 리턴값을 지정하지 않을 경우 생성된 객체가 리턴된다
   (this로 바인딩된 새로 생성된 객체가 리턴된다)

- 객체를 리턴했을 경우
```js
//Person생성자 함수
function Person(name, age, gender){
    this.name = name;
    this.age = age;
    this.gender = gender;

    //명시적으로 다른 객체 반환
    return {name:'bar', age:20, gender:'woman'};
}
var foo = new Person('foo', 30, 'man');
console.dir(foo);
```

- 기본타입(불린, 숫자, 문자열)을 리턴했을 경우
```js
function Person(name, age, gender){
    this.name = name;
    this.age = age;
    this.gender = gender;
    
    return 100;
}

var foo = new Person('foo', 30, 'man');
console.log(foo); //Person{name:"foo", age:30, gender:"man"}
```

## 4.5 프로토타입 체이닝
- 자바스크립트는 객체 리터럴, 생성자함수로 객체를 생성한다 이렇게 생성된 부모 객체가 프로토타입객체이다
  자식객체는 부모객체가 가진 프로퍼티 접근이나 메서드를 상속받아 호출하는것이 가능하다
- 모든 객체는 자신의 부모인 프로토타입 객체를 가리키는 참조 링크 형태의 숨겨진 프로퍼티가 있다
  이것을 암묵적 프로토타입 링크라고 부른다([[Prototype 링크]])
- 함수 객체의 prototype 프로퍼티와 객체의 숨은 프로퍼티인 [[Prototype]]링크를 구분해야 한다
- 모든 객체는 자신을 생성한 생성자 함수의 prototype 프로퍼티가 가리키는 프로토타입 객체를 자신의 부모 객체로 설정하는 [[Prototype]]링크로 연결한다

```js
//Person생성자 함수
function Person(name){
    this.name = name;
}
//foo객체 생성
var foo = new Person('foo');

console.dir(Person);
console.dir(foo);
```
=> prototype 프로퍼티나 [[Prototype]]링크는 같은 프로토타입 객체를 가리키고 있다
prototype 프로퍼티는 함수의 입장에서 자신과 링크된 프로토타입 객체를 가리키고,
[[Prototype]] 링크는 객체의 입장에서 자신의 부모 객체인 프로토타입 객체를 내부의 숨겨진 링크로 가르킨다

결국, 객체를 생성하는건 생성자 함수의 역할이지만, 생성된 객체의 실제 부모 역할을 하는건 prototype 프로퍼티가 가리키는 프로토타입 객체이다

## 4.5.2 프로토타입 체이닝
프로토타입 체이닝 : 객체는 자기 자신의 프로퍼티뿐만이아니라 자신의 부모 역할을 하는 프로토타입 객체의 프로퍼티 또한 마치 자시의 것처럼 접근하는 것이 가능하다
                   특정 객체의 프로퍼티나 메서등 접근하려고 할 때, 해당 객체에 접근하려는 프로퍼니 또는 메서드가 없다면 [[Prototype]]링크를 따라 
                   자신의 부모 역할을 하는 프로토타입 객체의 프로퍼티를 차례대로 검색한다
```js
var myObject = {
    name: 'foo',
    sayName: fucntion(){
        console.log('my Name is ' + this.name);
    }
};

myObject.sayName(); //My Name is foo
console.log(myObject.hasOwnProperty('name')); //true
console.log(myObject.hasOwnProperty('nickName')); //false
myObject.sayNickName(); //nomethod
```
- 객체 리터럴로 생성한 객체는 Object()라는 내장 생성자 함수로 생성된 것이다
- Object() 생성자 함수도 함수 객체이므로 prototype이란느 프로퍼티속성이 있다

## 4.5.3 생성자 함수로 생성된 객체의 프로토타입 체이닝
- 모든 객체는 자시을 생성한 생성자 함수의 prototype 프로퍼티가 가리키는 객체를 자신의 프로토타입 객체(부모객체)로 취급한다
- 리터럴 방식과 생성자 함수로 객체를 생성하는 방식이 프로토타입 체이닝 방법에서도 차이가 있다

```js
//Person생성자 함수
function Person(name, age, hobby){
    this.name = name;
    this.age = age;
    this.hobby = hobby;
}
//foo 객체 생성
var foo = new Person('foo', 30, 'tennis');

//프로토타입 체이닝
console.log(foo.hasOwnProperty('name')); // true

//Person.protorype 객체 출력
console.dir(Person.prototype);

```
