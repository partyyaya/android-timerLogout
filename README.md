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
- restartTimer => 使用於按鈕觸發時重新計算間(也可放在想要觸發的地方)
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



