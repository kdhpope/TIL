AsyncTask

Handler + Thread?

```java
//제너릭스 <parameter type, progess type ,return type>
class MyTask extends AsyncTask<Integer,Integer,String>{

        int cnt;

        public MyTask(int cnt) {
            this.cnt = cnt;
        }

        //thread가 동작되기 전
        @Override
        protected void onPreExecute() {
            progressBar.setMax(cnt);
            button.setEnabled(false);
            textView.setText("thread start");
        }

        //thread가 동작된 후
        @Override
        protected void onPostExecute(String s) {
            button.setEnabled(true);
            textView.setText(s);
        }

        //thread가 동작되는 중
        @Override
        protected void onProgressUpdate(Integer... values) {
            progressBar.setProgress(values[0].intValue());
            textView.setText(values[0].intValue()+"");
        }

        //Thread가 동작 되는 부분
        @Override
        protected String doInBackground(Integer... integers) {
            String result = "";
            int sum = 0;
            for(int i=0;i<cnt;i++){
                try {
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }publishProgress(i);
                result+= sum + " ";
            }
            return result;
        }
    }
```

시작

```java
MyTask mytask = new MyTask(50);
mytask.execute();
```

중지

```java
mytask.cancel(ture);
```

