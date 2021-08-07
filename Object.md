> java에서 OOP의 상속 중 Object클래스에 대한 내용을 정리한 포스팅입니다.    

<br/>  

# Object클래스  
- 모든 클래스의 최상위 조상 클래스


```java
class myClass{
    //...
}
// 사실 위와 같은 클래스는 다음과 같다
class myClass extends Object{
    //...
}
```  
- Object클래스에 인스턴스가 가져야 할 기본적인 (9+2)개의 메서드가 정의되어 있음 ( 가장 기본인 java.lang패키지 )   
<br/>  


|Object 메소드|설명|사용|   
|---|---|---|  
|**clone()**| 객체 자신의 복사본을 반환|protected Object clone|  
|**equals()**| 서로 같은 객체인지 확인 |public boolean equals(Object obj)|
|**getClass()**| 클래스 정보를 담고 있는 class 인스턴스를 반환 |public Class getClass()|
|**hashCode()**| 해시코드(메모리 주소값을 변환한 것)를 반환|public int hashCode()|  
|**toString()**| 정보를 문자열로 반환 |public String toString()|  
|finalize()| 객체 소멸시 GC에 의해 자동적으로 호출됨, 오버라이딩 해서 씀 |protected void finalize()|  
|notify()|현재 객체를 사용하려고 기다리는 쓰레드를 하나만 깨움|pubilc void notify()|  
|notifyAll()|현재 객체를 사용하려고 기다리는 모든 쓰레드를 깨움|pubilc void notifyAll()|  
|wait()|다른 쓰레드가 notify(),notifyAll()을 통해 깨우거나 인터럽트, 타임아웃등으로 깨울때 까지 기다리게 함 |pubilc void wait()|
* GC(Garbage Collection) : 프로그램이 동적으로 할당했던 메모리 영역 중에서 필요없게 된 영역을 해제하는 기능  
* wait()는 2개가 추가로 오버로딩 되어있음 → public void wait(long timeout), public void wait(long timeout,int nanos)   
<br/>  


```java
public boolean equals(Object obj){
    return (this==obj); 
} // 두 객체의 같고 다름은 참조변수의 값으로 판단함.
// 따라서 다른 두 객체를 equals로 비교하면 항상 false 
// String 은 pool을 이용하므로 true를 나타냄.

public String toString(){
    return getClass().getName()+"@"+Integer.toHexString(hashCode());
} // default상태는 위와 같으며 해싱처리된 메모리주소를 리턴함.
// 보통 오버라이딩 해서 사용 

class Point implements Cloneable { //clone은 Cloneable인터페이스를
                                // 구현한 클래스에서만 호출가능.
                                // (데이터 보호를 위해서)
    int x,y;
    Point(int x ,int y){
        this.x=x;this.y=y;
    }
    public Object clone(){
        Object obj = null;
        try{
            obj = super.clone(); // clone()은 예외처리가 필수
        } catch(CloneNotSupportedException e){}
        return obj;
    }
} //clone()은 단순히 인스턴스변수의 값만 복사 (얕은 복사) 하므로
// 오버라이딩을 통해 깊은 복사(완전히 새로운 객체가 만들어지게 함)가 이루어 지도록 해야함. 
```
