메인쓰레드를 서브스레드가 제어할 수 없다.

-->



1) runOnUiThread를 통해서 main Thread에 있는 Object를 컨트롤 할  수 있다.

```java
runOnUiThread(new Runnable() {    @Override    public void run() {        textView.setText(finalI+"");    }});
```



2) 핸들러를 사용한다.

전체 코드

```java
package com.example.p478;

import androidx.annotation.NonNull;
import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.os.Handler;
import android.os.Message;
import android.util.Half;
import android.util.Log;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;

public class MainActivity extends AppCompatActivity {
    TextView    textView;
    CountHandler countHandler;
    Button button;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        textView = findViewById(R.id.textView);
        countHandler = new CountHandler();
        button= findViewById(R.id.button);
    }
    Runnable t =new Runnable() {
        @Override
        public void run() {
            for(int i=1;i<10;i++){
                try {
                    Thread.sleep(500);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                Log.d("[T]","$$$$$$"+i);
            }
            Message message = countHandler.obtainMessage();
            Bundle bundle = new Bundle();
            bundle.putBoolean("end",true);  //데이터삽입
            message.setData(bundle);
            countHandler.sendMessage(message);

        }
    };
    class CountHandler extends Handler{
        @Override
        public void handleMessage(@NonNull Message msg) {
            Bundle bundle = msg.getData();
            if(bundle.getBoolean("end")) button.setEnabled(true);
        }
    }
    public void clickBt(View v){
        Thread thread = new Thread(t);
        thread.start();
        button.setEnabled(false);
    }
}

```



sub thread side

Bundle를 만들고, 데이터를 담아서, 메시지에 담고, 메시지를 보낸다.

```java
Message message = countHandler.obtainMessage();
Bundle bundle = new Bundle();
bundle.putBoolean("end",true);  //데이터삽입
message.setData(bundle);
countHandler.sendMessage(message);
```



main thread side

```java
class CountHandler extends Handler{
    //sub thread로부터 메시지가 오면 자동으로 메시지를 받는다.    
    @Override
    public void handleMessage(@NonNull Message msg) {
        Bundle bundle = msg.getData();
        if(bundle.getBoolean("end")) button.setEnabled(true);
    }
}

CountHandler countHandler;
countHandler = new CountHandler();
//객체가 생성된 순간부터 계속 메시지 수신을 대기중
```

2) - 1 다른 방식

하나만 간단하게 처리할 때

```java
package com.example.p478;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.os.Handler;
import android.util.Log;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;

public class MainActivity extends AppCompatActivity {
    TextView    textView;
    Button button;
    Handler handler;


    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        textView = findViewById(R.id.textView);
        button= findViewById(R.id.button);
        handler = new Handler();
    }
    Runnable t =new Runnable() {
        @Override
        public void run() {
            for(int i=1;i<10;i++){
                try {
                    Thread.sleep(500);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }

                Log.d("[T]","$$$$$$"+i);
            }
            //핸들러 작동
            handler.post(new Runnable() {
                @Override
                public void run() {
                    button.setEnabled(true);
                }
            });
        }
    };
    public void clickBt(View v){
        Thread thread = new Thread(t);
        thread.start();
        button.setEnabled(false);
    }
}

```

