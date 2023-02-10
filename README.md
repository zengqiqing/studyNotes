# studyNotes

Hello!

```javascript
class A{
    method1(){
        this.y = 2
    }
}

class B extends A {
    construtor(){
        super();
    }
    usualMethod(){
        super.x = 1;
        super.method1()
    }
}

var b = new B()
b.usualMethod()
console.log(b)

```

