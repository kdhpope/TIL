## 안드로이드 크기 단위

- in : 인치 

- mm : 밀리미터

- px : 빅셀

- dp : 밀도 독립적 픽셀(Density-independent Pixels)

  px = dp * (dpi/160) 

  디바이스마다 화면 크기가 다르기 때문에 크기를 일정한 수치에 고정시기고, 크기의 비율에 맞춰 크기를 키우거나 줄인다.

- sp : 배율 독립적 픽셀(scale-independant Pixels)

  dp에서 기본비율크기(160)을 사용자가 설정할 수 있다. 시스템에서 폰트에 대한 지원을 한다면(system에서 모든 글자크기를 키우는 등의 처리) 시스템폰트 설정에 따라서 커지거나 작아지거나 함

- pt : 포인트



## listener의 방식

1.  익명(Anonymous) 클래스를 생성하여 이벤트 리스너로 사용하기

```java
Button button1 = (Button) findViewById(R.id.button1) ;
    button1.setOnClickListener(new Button.OnClickListener() {
        @Override
        public void onClick(View view) {
            // TODO : click event
        }
    });
```

단점 : 익명 클래스에 정의된 이벤트 핸들러에서 익명 클래스 외부에 있는 변수를 사용하려면 변수를 선언할 때 반드시 final 키워드를 사용해야 한다는 것



 그러므로

- Button의 개수가 적거나, Button들 간의 연관성이 적은 경우.
- 이벤트 핸들러 함수 내에서 익명 클래스 외부의 변수를 참조하지 않는 경우.
- 간단한 Button 클릭 이벤트 테스트 코드를 작성하는 경우.

2. 생성해 놓은 익명(Anonymous) 클래스의 참조를 이벤트 리스너로 사용하기

```java
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        // ... 코드 계속

        Button.OnClickListener onClickListener = new Button.OnClickListener() {
            @Override
            public void onClick(View view) {
                TextView textView1 = (TextView) findViewById(R.id.textView1);
                switch (view.getId()) {
                    case R.id.buttonRed :
                        textView1.setText("Red") ;
                        textView1.setBackgroundColor(Color.rgb(255, 0, 0));
                        break ;
                    case R.id.buttonGreen :
                        textView1.setText("Green") ;
                        textView1.setBackgroundColor(Color.rgb(0, 255, 0));
                        break ;
                    case R.id.buttonBlue :
                        textView1.setText("Blue") ;
                        textView1.setBackgroundColor(Color.rgb(0, 0, 255));
                        break ;
                }
            }
        } ;
        Button buttonRed = (Button) findViewById(R.id.buttonRed) ;
        buttonRed.setOnClickListener(onClickListener) ;
        Button buttonGreen = (Button) findViewById(R.id.buttonGreen) ;
        buttonGreen.setOnClickListener(onClickListener) ;
        Button buttonBlue = (Button) findViewById(R.id.buttonBlue) ;
        buttonBlue.setOnClickListener(onClickListener) ;
    }
}
```

이벤트가 어디서 처리되는지 첫 번째 방법에 비해 직관적이지 않은 단점



그러므로

- Button이 여러 개 존재하고 Button들 간의 연관성이 많은 경우.
- 이벤트 핸들러 함수 내에서 익명 클래스 외부의 변수를 참조하지 않는 경우.
- 추후 또 다른 Button을 추가하여 사용할 가능성이 높은 경우.



3. 이벤트 리스너 인터페이스를 implements하는 이벤트 리스너 클래스 생성하기

```java
class BtnOnClickListener implements Button.OnClickListener {
        @Override
        public void onClick(View view) {
            ...
        }
}
```

## bundle

bundle : 앱이 살아있는 동안에 데이터를 잠시 저장해 두는곳?

앱이 백그라운드로 내려가면 유지해야하는 데이터만 번들에 저장되고, 나머지 프레임 등은 종료된다?

앱이 종료되면 사라진다.



##  SharedPreference

  처음 앱을 실행시켰을 때 저장할 내용을 적고 앱을 종료 후 다시 켰을 때 내용이 불러들여 진다

```java
if(view.getId() == R.id.button3){
            sp = getSharedPreferences("ma",MODE_PRIVATE);
            SharedPreferences.Editor editor = sp.edit();
            editor.putString("key01","id01");
            editor.commit();
        }
        else if(view.getId() == R.id.button4){
            sp = getSharedPreferences("ma", MODE_PRIVATE);
            String id = sp.getString("key01","default");
            Toast.makeText(this, id, Toast.LENGTH_SHORT).show();
        }
```

### Toast

내가 새로운 layout을 만들어서 toast로 뿌릴 수 있다.

```java
 LayoutInflater layoutInflater = getLayoutInflater(); //layoutInflater
 View tv =layoutInflater.inflate(R.layout.toast,(ViewGroup)findViewById(R.id.toastLayout)); //뷰의 생성
Toast toast = new Toast(this); //새로운 toast를 만듦
toast.setView(tv); //toast에 view를 올림
toast.setDuration(); //toast의 지속시간
toast.setGravity(); //toast의 위치
toast.show();
```



### Dialog

```java
AlertDialog.Builder alertBuilder = new AlertDialog.Builder(this);
alertBuilder.setTitle("my dialog");
alertBuilder.setMessage("my Message");
alertBuilder.setIcon(R.drawable.icon);
alertBuilder.setPositiveButton("OK", new DialogInterface.OnClickListener() {
    @Override
    public void onClick(DialogInterface dialogInterface, int i) {
    }
});
alertBuilder.setNegativeButton("NO", new DialogInterface.OnClickListener() {
	@Override
    public void onClick(DialogInterface dialogInterface, int i) {
    }
});
AlertDialog dialog = alertBuilder.create();
dialog.show();
```



### Progress

```java
pd = new ProgressDialog(MainActivity.this);
pd.setProgressStyle(ProgressDialog.STYLE_SPINNER);
pd.setMessage("progress...");
pd.setCanceledOnTouchOutside(false);
pd.setButton(pd.BUTTON_NEGATIVE, "cancle", new DialogInterface.OnClickListener() {
	@Override
	public void onClick(DialogInterface dialogInterface, int i) {
		dialogInterface.dismiss();
	}
});
pd.show();
```



### SeekBar 

progressbar하고 비슷하다.





참조 : https://recipes4dev.tistory.com/55

