CGI : 하나의 request당 하나의 process(servlet이 여러개?)

Gmarket 사용자가 들어올때마다 thread imvocatio



java에서 Thread의 구현

1) extends

```java
package day01;

public class Th1 {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		MyThread t1 = new MyThread("T1");
		MyThread t2 = new MyThread("T2");
		t1.start();
		t2.start();
	}

}
class MyThread extends Thread{
	String name;
	public MyThread(String name) {
		this.name = name;
	}
	@Override
	public void run() {
		
	}
	
}

```



2) implement



thread의 우선순위

```java
thread.setPriority(int newPriority);
```

같은 쓰레드가 다시 도는것을 방지하는 것

```java
public void run(){
    ~~~~
    yield();
}
```



ThreadGroup

연관된 쓰레드를 관리하기 위해 ThreadGroup란 기능이 있다.

```java
ThreadGroup tg1 = new ThreadGroup("TG1");
Thread t1 = new Thread(tg1, new Runnable() {
			
			@Override
			public void run() {
				// TODO Auto-generated method stub
				
			}
		});
```





지금까지 웹(표면에 보여주는 것, 통신 제외)은 하나의 thread였다.

html5에선 웹도 multi thread를 지원함

화면은 완성시켜 놓고 그 외의 기능은 나중에 추가(thread를 이용해서?)