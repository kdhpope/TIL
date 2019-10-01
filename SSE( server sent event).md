### SSE( server sent event)

client에서 request를 보내지 않아도, 일정 시간마다 server에서 client로 데이터를 보냄

- server

```java
protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.setContentType("text/event-stream"); //표시형식
		response.setCharacterEncoding("UTF-8");       //encoding
		
		PrintWriter pw = response.getWriter();
		String msg = server.getMessage();//보낼 데이터 입력
		pw.write("event: server-time\n\n");
		
		if(!msg.equals("")) {
			pw.write("data: "+msg+"\n\n"); // 받는 쪽은 msg만 받음
            // data 외의 태그는 불가능하다.
            // 그러므로 여러 데이터를 보내려면 data : JSON 형식으로 만들어야 함
			System.out.println("data send!!");
		}

		pw.close();
		
	}
```



- client

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
index
<script>
	var source = new EventSource('/IoTConnectMiniProject/Servlet'); //servlet 이름
	source.onmessage = function(e){
		document.body.innerHTML += e.data + '<br>'; //e.data에 데이터가 들어감
        											//위의 예시에서는 msg가 포함됨
	};
</script>
</body>
</html>
```

