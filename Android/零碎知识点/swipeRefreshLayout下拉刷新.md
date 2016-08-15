`swipeRefreshLayout`这个类是在放在 `android-support-v4.jar`里面的，这个`v4`一般默认创建工程的时候就会帮你加入，没有的话自己在工程中创建libs然后copy进入就可以。

####在XML中使用：

```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
  android:layout_width="match_parent"
  android:layout_height="match_parent"
  android:orientation="vertical" >

  <android.support.v4.widget.SwipeRefreshLayout
    android:id="@+id/refresh_layout"
    android:layout_width="match_parent"
    android:layout_height="match_parent" >

    <ListView
      android:id="@+id/listview"
      android:layout_width="match_parent"
      android:layout_height="match_parent"
      android:cacheColorHint="@android:color/transparent" >
    </ListView>
  </android.support.v4.widget.SwipeRefreshLayout>

</LinearLayout>
```

####在代码中处理

```
public class ListviewActivity extends Activity implements OnRefreshListener {

  private List<String> datas = new ArrayList<String>();//lis的数据
  
  private ListView listView = null;
  
  private SwipeRefreshLayout refresh_layout = null;//刷新控件

  private ArrayAdapter<String> adapter;
  
  @Override
  protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.listview_layout);
    listView  = (ListView) this.findViewById(R.id.listview);
    refresh_layout = (SwipeRefreshLayout) this.findViewById(R.id.refresh_layout);
    refresh_layout.setColorScheme(R.color.green, R.color.gray, R.color.blue_50, R.color.light_white);//设置跑动的颜色值
    adapter = new ArrayAdapter<String>(this, android.R.layout.simple_list_item_1,
        android.R.id.text1, datas);
    listView.setAdapter(adapter);
    for (int i = 0; i < 30; i++) {
      datas.add("item:"+i);
    }
    adapter.notifyDataSetChanged();
    refresh_layout.setOnRefreshListener(this);//设置下拉的监听
  }

  @Override
  public void onRefresh() {
    new Thread(new Runnable() {//下拉触发的函数，这里是谁1s然后加入一个数据，然后更新界面
      @Override
      public void run() {
        try {
          Thread.sleep(1000);
        } catch (InterruptedException e) {
          e.printStackTrace();
        }
        datas.add(0,"item:refresh...");
        handler.sendEmptyMessage(0);
      }
    }).start();
  }
  
  private MyHandler handler = new MyHandler();
  
  class MyHandler extends Handler{
    @Override
    public void handleMessage(Message msg) {
      switch (msg.what) {
      case 0:
        refresh_layout.setRefreshing(false);
        adapter.notifyDataSetChanged();
        break;
      default:
        break;
      }
    }
  }
  
}
```