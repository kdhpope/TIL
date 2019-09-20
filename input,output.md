## I/O

i/o는 거의 대부분 thread를 쓴다.

### stream

`데이터의 통로`

network와 file이 대표적

하지만 그 외 프로세스와 프로세스 사이에서도 스트림 통신이 가능한다.

stream을 이용하기 위해서 통로로 port를 사용한다.

한번에 대량의 데이터를 송, 수신할때는 버퍼를 사용함



#### network stream

#### file stream



buffer를 사용하면 빠르다.

하지만 버퍼에 사용하지 않은 잔재가 남아있으면 오류를 일으킬수 있으므로 buffer를 닫을 때,

flush()를 통해 비워 준 다음, close()를 해야 한다.



java - fileInputStream 예제

byte로 받는다. 그러므로 read가 int만 가능

```java
package day03;

import java.io.FileInputStream;
import java.io.IOException;

public class Fi1 {

	public static void main(String[] args)  {
		// TODO Auto-generated method stub
		FileInputStream fi = null;
		try {
			fi = new FileInputStream("test.txt");
			int data =0;
			while((data=fi.read())!=-1) {
				System.out.print(data);
			}
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		finally {
			if(fi != null) {
				try {
					fi.close();
				} catch (IOException e) {
					// TODO Auto-generated catch block
					e.printStackTrace();
				}
			}
		}
	}

}
```



bufferReader 예제

String으로 받을 수 있다.

```java
package day03;

import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class Fi1 {

	public static void main(String[] args)  {
		// TODO Auto-generated method stub
		FileReader fi = null;
		BufferedReader br = null;
		try {
			fi = new FileReader("test.txt");
			br = new BufferedReader(fi);
			String data = "";
			while((data = br.readLine())!=null) {
				System.out.print(data);
			}
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		finally {
			if(fi != null) {
				try {
					fi.close();
				} catch (IOException e) {
					// TODO Auto-generated catch block
					e.printStackTrace();
				}
			}
		}
	}

}

```



bufferStream 예제

BufferedInput/outputStream을 사용하면 close 할때 자동으로 FileInput/OutputStream을 close해준다

```java
package day03;

import java.io.BufferedInputStream;
import java.io.BufferedOutputStream;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;

public class Fi1 {

	public static void main(String[] args) throws IOException  {
		// TODO Auto-generated method stub
		FileInputStream fis = null;
		BufferedInputStream bis = null;
		FileOutputStream fos = null;
		BufferedOutputStream bos = null;
		try {
			fis = new FileInputStream("test.txt");
			bis = new BufferedInputStream(fis);
			
			fos = new FileOutputStream("text.txt");
			bos = new BufferedOutputStream(fos);
			
			int data = 0;
			while((data=bis.read())!= -1) {
				bos.write(data);	
			}
			
		} catch (FileNotFoundException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		if(bis != null) {
			bis.close();
		}
		if(bos != null) {
			bos.flush();
			bis.close();
		}
	}

}

```





### Socket programming

Server

```java
package Day04;

import java.io.BufferedWriter;
import java.io.IOException;
import java.io.OutputStream;
import java.io.OutputStreamWriter;
import java.net.ServerSocket;
import java.net.Socket;

public class Server {
	int port;
	ServerSocket serverSocket;
	Socket socket;
	
	OutputStream out;
	OutputStreamWriter osw;
	BufferedWriter bw;
	
	public Server(int port) throws IOException {
		this.port = port;
		
		serverSocket = new ServerSocket(port);
	}
	public void runServer() throws IOException {
		System.out.println("Server start");
		try {
			System.out.println("Server ready");
			socket = serverSocket.accept();
			System.out.println("Accepted..."+socket.getInetAddress());
			
			out = socket.getOutputStream();
			osw = new OutputStreamWriter(out);
			bw = new BufferedWriter(osw);
			bw.write("hello");
		} catch (IOException e) {
			// TODO Auto-generated catch block
			throw e;
		}finally {
			if(bw != null) {
				bw.close();
			}
			if(socket !=null) {
				socket.close();
			}
		}

		System.out.println("server close");
	}
	
	public static void main(String[] args) {
		// TODO Auto-generated method stub
		Server server = null;
		
		try {
			server = new Server(8888);
			server.runServer();
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}

}

```



Client

```java
package Day04;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.net.Socket;
import java.net.UnknownHostException;

public class Client {
	
	String ip;
	int port;
	
	Socket socket;
	InputStream is;
	InputStreamReader isr;
	BufferedReader br;
	public Client(String ip, int port) {
		this.ip = ip;
		this.port = port;
	}

	public void connect() {
		try {
			socket = new Socket(ip,port);
		} catch (UnknownHostException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}
	public void request() throws IOException {
		try {
			is = socket.getInputStream();
			isr = new InputStreamReader(is);
			br = new BufferedReader(isr);
			
			String str = br.readLine();
			System.out.println(str);
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}finally {
			if(is != null) {
				is.close();
			}
			if(isr != null) {
				isr.close();
			}
			if(br != null) {
				br.close();
			}
			if(socket != null) {
				socket.close();
			}
		}
	}

	public static void main(String[] args) throws IOException {
		// TODO Auto-generated method stub
		Client client = null;
		client = new Client("70.12.60.92",8888);
		client.connect();
		client.request();
		
	}

}

```

