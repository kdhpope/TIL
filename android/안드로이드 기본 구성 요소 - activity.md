안드로이드 네 가지 기본 구성 요소(Activity, service, broadcast receiver, content provider)

1. activity

   화면과 함께 기능

2. service

   화면에 안보여주지만 돌아가는 기능?

3. broadcast receiver

   OS가 뿌려주는 여러 정보를 받는 것(배터리가 부족함 등등) (OS가 broadcaster, app이 receiver)

4. content provider

   app에서 당사자app밖의 정보를 사용하게 해주는 것(ex : app에서 갤러리의 사진을 이용한다. )



### activity에서 다른 activity로

새로운 activity를 만들고

xml에 새 activity를 등록 해야 한다.

```xml
<application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">
        <activity android:name=".Main2Activity"></activity>
        <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>
```

새 액티비티 : Main2Activity, 기존 activity : MainActivity

intent : 앱과 앱 간의 연결을 담당

```java
//Intent(start.this, dest.class)의 형식으로
    
Intent intent = new Intent(MainActivity.this,Main2Activity.class);
intent.putExtra("num",100);
intent.putExtra("txt",textView.getText());
startActivity(intent);

Intent intent = getIntent();
String imgName = intent.getStringExtra("txt");
```

1. 출발지와 목적지 설정

2,3 : 데이터를 (key,value)로 삽입

4 : 목적지 액티비티로 출발



5~6: dest activity에서 데이터를 받는법



액티비티가 중지될때 데이터를 저장,복구,초기화 하는 방법

```java
protected void restoreState(){
        SharedPreferences pref = getSharedPreferences("pref", Activity.MODE_PRIVATE);
        if((pref!=null) && (pref.contains("id") && pref.contains("pw"))){
            id.setText(pref.getString("id",""));
            pw.setText(pref.getString("pw",""));
            login();
        }
    }
    protected void saveState(){
        SharedPreferences pref = getSharedPreferences("pref", Activity.MODE_PRIVATE);
        SharedPreferences.Editor editor = pref.edit();
        editor.putString("id",id.getText().toString());
        editor.putString("pw",pw.getText().toString());
        editor.commit();
    }
    protected void clearState(){
        SharedPreferences pref = getSharedPreferences("pref", Activity.MODE_PRIVATE);
        SharedPreferences.Editor editor = pref.edit();
        editor.clear();
        editor.commit();
    }
```



액티비티 A로부터 B로 간다음, B로부터 A로 데이터를 넘기는 방법

```java
A
startActivityForResult(intent,101);

@Override
protected void onActivityResult(int requestCode, int resultCode, @Nullable Intent data) {
    super.onActivityResult(requestCode, resultCode, data);
    if(requestCode == 101) {
    	//action;
    }
}


B
 Intent i = new Intent();
if(v.getId()==R.id.goMenuButton){
	i.putExtra("order","menu");
}
else if(v.getId()==R.id.goLoginButton){
	i.putExtra("order","login");
}
setResult(RESULT_OK,i);
finish();
```





### Fragment

```java
public class MainActivity extends AppCompatActivity {
    view_1Fragment view_1Fragment;
    View_2Fragment view_2Fragment;
    View_3 view_3;


    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        view_1Fragment = new view_1Fragment();
        view_2Fragment = new View_2Fragment();
        view_3 = new View_3();
    }
    public void clickViewButton(View v){
        if(v.getId() == R.id.button){
            onFragmentChange(1);
        }
        if(v.getId() == R.id.button2){
            onFragmentChange(2);
        }
        if(v.getId() == R.id.button3){
            onFragmentChange(3);
        }
    }
    public void onFragmentChange(int index){
        if(index==1){
            getSupportFragmentManager().beginTransaction().replace(R.id.fragmentContainerLayout,view_1Fragment).commit();
        }
        else if(index == 2){
            getSupportFragmentManager().beginTransaction().replace(R.id.fragmentContainerLayout,view_2Fragment).commit();
        }
        else if(index == 3){
            getSupportFragmentManager().beginTransaction().replace(R.id.fragmentContainerLayout,view_3).commit();
        }
    }
}
```



