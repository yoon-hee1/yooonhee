# 객체지향 프로그래밍

## 6.2.2 클래스 기반의 상속
```js
function Person(arg) {
	this.name = arg;
}

Person.prototype.getName = function() {
	return this.name;
}

Person.prototype.setName = function(value) {
	this.name = value;
}

function Student(arg) {
}

var you = new Person("iamhjoo");
Student.prototype = you;

var me = new Student("zzoon");
me.setName("zzoon");
console.log(me.getName());
```
이 함수 객체의 프로토타입을 하여금 Person 함수 객체의 인스턴스를 함조
Student 함수 객체로 생성된 객체 me의 [[Prototype]] 링크가 생성자의 프로토타입 프로퍼티 Student.prototype인 you를 가르키고
new Person()으로 만들어진 객체의 [[Prototype]] 링크는 Person.prototype을 가르키는 프로토타입 페인이 형성된다
-> 그러나 위 예제는 me 인스턴스를 생성할 때 부모 클래스인 Person의 생성자를 호출하지 않는다
![6-7](https://user-images.githubusercontent.com/53419312/68996734-757cd300-08e1-11ea-9a2d-8dcd5895e1b0.PNG)

```js
var me = new Student("zzoon");
```
이 코드로 me 인스턴스를 생성할 때 "zoon"을 인자로 넘겼으나, 이를 반영하는 코드는 없다
-> 이렇게 부모의 생성자가 호출되지 않으면, 인스턴스의 초기화가 제대로 이루어지지않아 문제가 발생한다
   student 함수에 다음 코드를 추가하여 부모 클래스의 생성자를 호출해야 한다

```js
function Student(arg){
    Person.apply(this.arguments);
}
```
이런 식으로 자식 클래스의 인스턴스에 대해서도 부모 클래스의 생성자를 실행시킬 수 있다. 
클래스간의 상속에서 하위클래스의 인스턴스를 생성 할 때, 부모클래스의 생성자를 호출 하는 경우에 필요한 방식이다
![6-8](https://user-images.githubusercontent.com/53419312/68996746-95ac9200-08e1-11ea-9336-b443b5a51fee.PNG)

```js
function Person(arg) {
    this.name = arg;
}

Function.prototype.method = function(name, func) {
    this.prototype[name] = func;
}

Person.method("setName", function(value) {
    this.name = value;
});
Person.method("getName", function() {
    return this.name;
});

function Student(arg) {
}

function F() {};
F.prototype =Person.prototype;
Student.prototype = new F();
Student.prototype.constructor = Student;
Student.super = Person.prototype;

var me = new Student();
me.setName("zzoon");
console.log(me.getName());
```
![6-9](https://user-images.githubusercontent.com/53419312/68996754-a65d0800-08e1-11ea-9888-cb9b72ffdefe.PNG)
빈 함수의 객체를 중간에 두어 Person의 인스턴스와 Student의 인스턴스를 서로 독립적으로 만들었다
이제 Person 함수 객체에서 this에 바인딩 되는 것은 Student의 인스턴스가 접근 할 수 없다

```js
var inherit = function(Parent, Child){
    var F = function() {};
    return function(Parent, Child){
        F.prototype = Parent.prototype;
        Child.prototype = new F();
        Child.prototype.constructor = Child;
        Child.super = Parent.prototype;
    }
}
```
클로저는 F()함수를 지속적으로 함조한다
F()의 새성은 단 한번 이루어지고 inherit함수가 계속 호출되어도 함수 F()의 생성을 새로 할 필요가 없다

## 6.3 캡슐화
- 캡슐화 : 기본적으로 관련된 여러 가지 정보를 하나의 틀 안에 담는 것
  (멤버 변수와 메서드가 서로 관련된 정보가 되고, 클래스가 이것을 담는 하나의 큰 틀 이라고 할 수 있다)
```js
var Person = function(arg) {
	var name = arg ? arg : "zzoon" ;

	this.getName = function() {
		return name;
	}
	this.setName = function(arg) {
		name = arg;
	}
}; 

var me = new Person();
console.log(me.getName());
me.setName("iamhjoo");
console.log(me.getName());
console.log(me.name); // undefined
```
this 객체의 프로퍼티를 선언하면 외부에서 new 키워드로 생성한 객체로 접근 할 수있다
var로 선언된 멤버들은 외부에서는 접근이 불가능하다

```js
 var Person = function(arg) { 
	var name = arg ? arg : "zzoon" ;
	
	return {
		getName : function() {
			return name;
		},
		setName : function(arg) {
			name = arg;
		}
	};
 }
 
 var me = Person(); /* or var me = new Person(); */
 console.log(me.getName());
```
Person함수를 호출하여 객체로 반환받는다
사용자는 반환받는 객체로 메서드를 호출 할 수 있고, private멤버에 접근 할 수 있다
밑의 예제가 접근하는 private 멤버가 객체나 배열이면 얕은 복사로 참조만을 반환하므로 사용자가 이후 이를 쉽게 변경할 수 있다는 문제점을 보여준다
```js
 var ArrCreate = function(arg) { 
	var arr = [1,2,3];
	
	return {
		getArr : function() {
			return arr;
		}
	};
 }
 
 var obj = ArrCreate(); /* or var me = new Person(); */
 var arr = obj.getArr();
 arr.push(5);
 console.log(obj.getArr()); // [ 1,2,3,5 ]
```
밑의 예제는 객체를 반환하는 것이 아닌 함수를 반환한다
```js
 var Person = function(arg) { 
	var name = arg ? arg : "zzoon" ;
	
	var Func = function() {}
	Func.prototype = {
		getName : function() {
			return name;
		},
		setName : function(arg) {
			name = arg;
		}
	};
	
	return Func;
 }();
 
 
 var me = new Person();
 console.log(me.getName());
```
클로저를 활용하여 name에 접근 할 수 업세 했다
즉시 실행 함수에서 반환되는 Func이 클로저가 되고 이 함수가 참조하는 name 프로퍼티가 자연 변수가 된다
사용자는 name에 대한 접근이 불가능하다