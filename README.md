## 逾時登出(use timer to logout)
- 此功能可用於多頁與逾時管理
- 請先建立登入與登出機制畫面

#### 建立功能
- 1.在登入後主頁面增加 CountDownTimer
```java
private static CountDownTimer timer;
private static Context logoutContext;

@Override
protected void onCreate(Bundle savedInstanceState) {
  super.onCreate(savedInstanceState);
  setContentView(R.layout.activity_main);
  
  restartTimer();//開始計時(也可放入登入按鈕onclick事件)
  setLogoutActivity(this);//不能用getApplicationContext()
}

public static void restartTimer(){
  if(timer == null){
    //30分鐘
    timer = new CountDownTimer(30 *60 * 1000, 1000){
      public void onTick(long millisUntilFinished) {
   
      }

      public void onFinish() {
        finishLogout();
      }
    }.start();
  }else{
    //重新計時
    timer.cancel();
    timer.start();
  }
}

//當計時結束自動登出
public static void finishLogout(){
  SystemUtil.showTimerLogoutDialog(logoutContext);
}

//設定計時完要登出的當下頁面
public static void setLogoutActivity(Context context){
  logoutContext = context;
}
```
#### 其他頁面設定
- restartTimer => 使用於按鈕觸發時重新計算間(也可放在想要觸發的地方,ex:OnTouchListener)
- setLogoutActivity(this) => 使用於當下頁面逾時登出

```java
@Override
protected void onCreate(Bundle savedInstanceState) {
  super.onCreate(savedInstanceState);
  setContentView(R.layout.activity_main);
  
  setLogoutActivity(this);//基本上放開頭即可
}

/**
 * 設定按鈕按下動作
 */
public void onClick(View v) {
  restartTimer();
}
```

另提供 SystemUtil:
```java
/**
 * 顯示登出確認對話框
 */
public static void showTimerLogoutDialog(final Context from){
  //Create a builder
  AlertDialog.Builder builder = new AlertDialog.Builder(from);
  builder.setTitle("連線已逾時");
  builder.setMessage("連線已逾時，請重新登入");
  builder.setNegativeButton("確定",new DialogInterface.OnClickListener() {
        public void onClick(DialogInterface dialog, int which) {
          logout(from);
        }
      }
  );
  //Create the dialog
  AlertDialog ad = builder.create();
  //show
  ad.show();
  //resize alert window
  ad.getWindow().setLayout(600, 350);
}

/**
 * 登出
 */
public static void logout(Context from) {
  System.gc();
  Intent intent = new Intent();
  intent.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP);//清除所有activity歷史紀錄
  intent.setClass(from, LoginActivity.class);
  from.startActivity(intent);
}
```



