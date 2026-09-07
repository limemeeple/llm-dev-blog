---
title: "11. JavaScript OOP"
date: 2026-08-19
draft: false
tags: ["OOP"]
categories: ["STUDY"]
summary: "이어드림2026 서비스 개발 수업 정리"
weight: 11
---
## OOP란?
- Object-Oriented Programming
- 프로그램을 설계할 때 객체 단위로 파악하고, 이들 간의 상호작용을 통해 기능을 구현하는 프로그래밍 패러다임.

### 객체(Object)란 ?
- 객체란 상태와 동작을 가진 사람이나 사물이다.
- 일상에서 마주하는 책상, 사람, 자동차 등을 모두 객체라고 볼 수 있다.
- 즉, OOP란 우리가 일상 속에서 사물과 사람을 바라보는 시선으로 프로그래밍을 바라보는 것!

### OOP의 4가지 특성
1. 추상화
2. 캡슐화
3. 상속성
4. 다형성

#### OOP의 특성: 추상화(Abstraction)
- 공통된 특징들을 도출해서 묶는 것이 추상화다.

#### OOP의 특성: 캡슐화(Encapsulation)
- 객체를 독립적으로 분리시키고, 외부에서 데이터와 코드에 접근하지 못하도록 하는것이 캡슐화이다.

#### OOP의 특성: 상속성(Inheritance)
- 다른 클래스의 특징을 그대로 물려받아 새로운 클래스를 만든 것이 상속성이다.

#### OOP의 특성: 다형성(Polymotphism)
- 하나의 이름을 가진 변수, 함수, 클래스가 다양한 의미로 해석될 수 있는 것이 다형성이다.

### 클래스(Class)
- 클래스는 객체가 어떤 상태, 어떤 동작을 가져야 하는지에 대한 설계도, 설명서이다.

### 인스턴스(Instance)
- 인스턴스는 Class에 작성된 내용을 토대로 만들어진 실체화된 객체이다.

### 클래스와 인스턴스
```
class Person{

}

const elice = new Person();
```
- 클래스를 선언할 때에는 'class' 키워드를 사용한다.
- 해당 클래스의 인스턴스를 생성할 때에는 'new' 키워드를 사용한다.

### 프로퍼티(Property)
- 클래스는 상태와 동작을 가지고 있다. 이때, 상태에 해당하는 것이 바로 프로퍼티이다.
```
class Car{
    name;
    speed;
    wheels = 4;
}

const casper = new Car();
casper.name = "Casper";
casper.speed = 200;

console.log(casper.name);   // 'Casper'
console.log(casper.speed);  // 200
console.log(casper.wheels); // 4
```
- 인스턴스의 프로퍼티에 접근하려면 객체와 동일하게 프로퍼티 접근자('.')를 사용한다.

### 메서드(Method)
- 메서드란, 객체가 행할 수 있는 동작 대한 내용을 코드로 작성해 놓은 것이다.
```
class Car{
    name;
    speed;
    wheels = 4;

    drive(){
        console.log("driving...");
    }
}

const casper = new Car();
casper.drive(); // 'driving...'
```
- 클래스 내부에서 일반 함수를 작성하는 것과 비슷하게 선언한다.

### 생성자(Constructor)
- 객체도 처음 생성될 때 필요한 기본 정보를 세팅하는 과정이 필요한데 생성자 메서드를 통해 객체 생성시에 필요한 초기 세팅 / 동작을 정의할 수 있다.
- 'constructor'라는 이름의 메서드를 선언하면 해당 메서드가 생성자가 된다.
<생성자 실습 코드>
```
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>JAVA SCRIPT OOP</title>
    </head>
    <body>

    </body>
    <script>
        class Car{
            name;
            speed;
            wheels = 4;
            /*
            init(self,name,speed):
                self.name = name
                self.speed = speed
            */
            constructor(name,speed){
                this.name = name;
                this.speed = speed;
            }

            printThis(){
                let name = 'local name'
                console.log(this);
                console.log(this.name);
                console.log(name);
            }

            drive(){
                console.log('driving!!!!');
                //printThis(); {} 내부에서 찾는다.
                this.printThis(); // this, 즉 이 객체 내부에서 찾는다.
            }
        }

        // 일반적인 객체화
        // 객체화 == 생성자 호출(생성자는 너무 당연한 것이기 때문에 생략이 가능)
        const car = new Car('빵빵이',200); 
        car.printThis();
        car.drive();

        car.name = '붕붕이';
        car.printThis();

    </script>
</html>
```
- 'constructor' 내부에서는 프로퍼티 선언 및 초기화, 그 외의 초기화 작성 등이 이루어진다.
- 생성자는 'new' 키워드를 이용한 인스턴스 생성 시에 자동으로 호출된다.

#### 기본 생성자
- 'constructor' 메서드를 선언하지 않아도 인스턴스 생성은 가능하다.
- 생성자가 생략되면 비어있는 'construcotr'가 기본으로 생성 되기 때문이다.
