use action bar

FragmentCallback 





## SERVICE

앱이 백그라운드(메모리)에 남아 있을 때 동작되는 것들?



서비스의 start와 bind

start는 시작만 하고 이후의 작동은 액티비티가 컨트롤 할 수 없지만, bind(ibind)에서는 계속 통신이 가능함.

bind는 start가 아니라서 onCreate, onStartCommand가 작동하지 않는다. 



Activity

```java
//새로운 서비스를 만들고, bind시킴
//MyService는 새로운 service페이지를 만들면 새로운 service가 새로운 객체로 만들어지고, 그 객체를 선언한다. 
ServiceConnection con = new ServiceConnection() {
        @Override
        public void onServiceConnected(ComponentName componentName, IBinder iBinder) {
            MyService.myBinder mb = (MyService.myBinder)iBinder;
            ms = mb.getService();
        }

        @Override
        public void onServiceDisconnected(ComponentName componentName) {

        }
    };
	//service로부터 받는 데이터를 받는 함수.
    @Override
    protected void onNewIntent(Intent intent) {
        super.onNewIntent(intent);
    }

@Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
		//해당하는 서비스를 만듦
        Intent intent = new Intent(getApplicationContext(),MyService.class);
        //service bind
        bindService(intent,con, Context.BIND_AUTO_CREATE);
    }
```



Service



서비스의 코드

```java
package com.example.p360;

import android.app.Service;
import android.content.Intent;
import android.os.Binder;
import android.os.IBinder;
import android.util.Log;

public class MyService extends Service {
    Intent intentkk;
    public MyService() {

    }
    boolean bt1action = false;
    boolean bt2action = false;

    class myBinder extends Binder{
        public MyService getService(){
            return MyService.this;
        }
    }
    IBinder iBinder = new myBinder();

    @Override  //service가 실행되면 가장 먼저 시작되는 함수
    public int onStartCommand(Intent intent, int flags, int startId) {
        Log.d("start","on start");
        return super.onStartCommand(intent, flags, startId);
    }

    public void bt(){
        Log.d("click","bt");
        Runnable run = new Runnable() {
       		@Override
            public void run() {
                    
            }
       	};
            Thread thread = new Thread(run);
            thread.start();
        }
    }
    @Override
    public IBinder onBind(Intent intent) {
        // TODO: Return the communication channel to the service.
        intentkk = new Intent(getApplicationContext(),MainActivity.class);
        intentkk.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK |
                Intent.FLAG_ACTIVITY_SINGLE_TOP |
                Intent.FLAG_ACTIVITY_CLEAR_TOP);

        return  iBinder;
    }
}

```





액티비티에서의 서비스 시작

```java
intent = new Intent(this,MyService.class);
startService(intent);
```





## broadcast Receiver

메시지를 여러 객체에 전달하는 것

관련 클래스는 BroadcastReceiver클래스를 상속 받아야 함

ex) 메시지를 받고, 보여주는 앱

```java
public class SmsReceiver extends BroadcastReceiver {
    public static final String TAG = "SmsReceiver";

    public SimpleDateFormat format = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");

    @Override
    public void onReceive(Context context, Intent intent) {
        Log.i(TAG, "onReceive() 메소드 호출됨.");

        Bundle bundle = intent.getExtras();
        SmsMessage[] messages = parseSmsMessage(bundle);
        if (messages != null && messages.length > 0) {
            String sender = messages[0].getOriginatingAddress();
            Log.i(TAG, "SMS sender : " + sender);

            String contents = messages[0].getMessageBody().toString();
            Log.i(TAG, "SMS contents : " + contents);

            Date receivedDate = new Date(messages[0].getTimestampMillis());
            Log.i(TAG, "SMS received date : " + receivedDate.toString());

            sendToActivity(context, sender, contents, receivedDate);
        }
    }

    private SmsMessage[] parseSmsMessage(Bundle bundle) {
        Object[] objs = (Object[]) bundle.get("pdus");
        SmsMessage[] messages = new SmsMessage[objs.length];

        int smsCount = objs.length;
        for (int i = 0; i < smsCount; i++) {
            if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M) {
                String format = bundle.getString("format");
                messages[i] = SmsMessage.createFromPdu((byte[]) objs[i], format);
            } else {
                messages[i] = SmsMessage.createFromPdu((byte[]) objs[i]);
            }
        }

        return messages;
    }

    private void sendToActivity(Context context, String sender, String contents, Date receivedDate) {
        Intent myIntent = new Intent(context, SmsActivity.class);
        myIntent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK |
                Intent.FLAG_ACTIVITY_SINGLE_TOP | Intent.FLAG_ACTIVITY_CLEAR_TOP);
        myIntent.putExtra("sender", sender);
        myIntent.putExtra("contents", contents);
        myIntent.putExtra("receivedDate", format.format(receivedDate));
        context.startActivity(myIntent);
    }
}
```





### Permission에 관하여



모든 Permission에 관하여 아래의 항목을 수행해야 하는 것은 아니다.

'위험 권한'에 대하여만 하면 됨



manifest

```xml
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>
```



activity

```java
public class MainActivity extends AppCompatActivity {
    String[] Permissions = {
            Manifest.permission.READ_EXTERNAL_STORAGE,
            Manifest.permission.WRITE_EXTERNAL_STORAGE
    };
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        
        ActivityCompat.requestPermissions(this,Permissions,101);//실제로 물어보는 코드

    }
    
    public void ActionRequiredPermission(){
        //권한이 필요한 액션이 실행될 때, 권한이 있는지 묻고 실행한다. 
        //지금 예에서는 phone call
        int permission = PermissionChecker.checkSelfPermission(this, Manifest.permission.CALL_PHONE);
        if(permission ==PackageManager.PERMISSION_GRANTED){
            Intent intent = new Intent(Intent.ACTION_CALL, Uri.parse("tel:010-1213-1415"));
            startActivity(intent);
        }
    }
}
```

