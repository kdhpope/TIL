

listview

나만의 adapter를 만들음

```java
package com.example.p427;

import androidx.appcompat.app.AppCompatActivity;
import androidx.core.app.ActivityCompat;
import androidx.core.content.PermissionChecker;

import android.Manifest;
import android.content.Context;
import android.content.Intent;
import android.content.pm.PackageManager;
import android.media.Image;
import android.net.Uri;
import android.os.Bundle;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.BaseAdapter;
import android.widget.ImageView;
import android.widget.LinearLayout;
import android.widget.ListView;
import android.widget.TextView;

import org.w3c.dom.Text;

import java.util.ArrayList;

public class MainActivity extends AppCompatActivity {
    ArrayList<Item> list;
    ListView listView;
    LinearLayout container;
    ItemAdapter itemAdapter;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        listView = findViewById(R.id.listViewx);
        container = findViewById(R.id.container);

        String[] Permissions = {
                Manifest.permission.CALL_PHONE,
        };
        ActivityCompat.requestPermissions(this, Permissions, 101);

    }
    class ItemAdapter extends BaseAdapter{
        ArrayList<Item> 리스트;

        public ItemAdapter() {
        }

        public ItemAdapter(ArrayList<Item> 리스트) {
            this.리스트 = 리스트;
        }

        @Override
        public int getCount() {
            return 리스트.size();
        }

        @Override
        public Object getItem(int i) {
            return 리스트.get(i);
        }
        public void addItem(Item item){
            리스트.add(item);
        }

        @Override
        public long getItemId(int i) {
            return i;
        }

        @Override
        public View getView(int i, View view, ViewGroup viewGroup) {
            View myView = null;
            LayoutInflater inflater = (LayoutInflater)getSystemService(Context.LAYOUT_INFLATER_SERVICE);
            myView = inflater.inflate(R.layout.layout,container,true);
            ImageView img1 = myView.findViewById(R.id.imageView);
            img1.setImageResource(리스트.get(i).getImageId());
            final int ia = i;

            TextView tv1 = myView.findViewById(R.id.textView);
            tv1.setText(리스트.get(i).getName());

            TextView tv2 = myView.findViewById(R.id.textView2);
            tv2.setText(리스트.get(i).getPhone());

            return myView;
        }
    }

    public void clickBt(View v){
        getData();
        itemAdapter = new ItemAdapter(list);
        listView.setAdapter(itemAdapter);
    }
    public void clickBt2(View v){
        itemAdapter.addItem(new Item("이선달","080-8383-1929",R.drawable.djinn));
        itemAdapter.notifyDataSetChanged();
    }

    private void getData(){
        list = new ArrayList<Item>();
        list.add(new Item("홍길동","010-1234-4321",R.drawable.ogre));
        list.add(new Item("김선비","010-1234-4321",R.drawable.oni));
        list.add(new Item("강백정","010-1234-4321",R.drawable.orc_head));

    }
}

```







spinner

```java
package com.example.p436;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.view.View;
import android.widget.AdapterView;
import android.widget.ArrayAdapter;
import android.widget.ImageView;
import android.widget.RatingBar;
import android.widget.Spinner;
import android.widget.Toast;

import java.util.ArrayList;

public class MainActivity extends AppCompatActivity implements AdapterView.OnItemSelectedListener {
    ArrayList<Integer> list;
    Spinner spinner;
    ImageView imageView;
    RatingBar ratingBar;
    int[] ratlist;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        spinner = findViewById(R.id.spinner);
        getData();
        ArrayAdapter<Integer> adapter = new ArrayAdapter<Integer>(this,android.R.layout.simple_spinner_item,list);
        adapter.setDropDownViewResource(android.R.layout.simple_spinner_dropdown_item);
        spinner.setAdapter(adapter);
        spinner.setOnItemSelectedListener(this);
        imageView = findViewById(R.id.imageView);
        ratingBar = findViewById(R.id.ratingBar);
        ratingBar.setMax(5);
        ratingBar.setStepSize(1);
    }

    private void getData() {
        list= new ArrayList<Integer>();
        list.add(R.drawable.alien_bug);
        list.add(R.drawable.alien_egg);
        list.add(R.drawable.anubis);
        list.add(R.drawable.bad_gnome);
        list.add(R.drawable.beast_eye);
        list.add(R.drawable.behold);
        list.add(R.drawable.bestial_fangs);
        list.add(R.drawable.bottled_shadow);
        list.add(R.drawable.brain_tentacle);
        list.add(R.drawable.brute);
        list.add(R.drawable.bully_minion);
        list.add(R.drawable.carnivorous_plant);
        list.add(R.drawable.ceiling_barnacle);
        list.add(R.drawable.centaur);
        ratlist = new int[list.size()];
        for(int i=0;i<ratlist.length;i++){
            ratlist[i] = 0;
        }
    }

    @Override
    public void onItemSelected(AdapterView<?> adapterView, View view, int i, long l) {
        int str = list.get(i);
        Toast.makeText(this, ""+str, Toast.LENGTH_SHORT).show();
        imageView.setImageResource(str);
        ratingBar.setRating(ratlist[i]);
        ratlist[i]++;
    }

    @Override
    public void onNothingSelected(AdapterView<?> adapterView) {

    }
}

```

