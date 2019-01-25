## 逾時登出(use timer to logout)
- 此功能可用於多頁與逾時管理
- 請先建立登入與登出機制畫面

#### 建立功能
- 1.在登入後主頁面增加 CountDownTimer
```
private static CountDownTimer timer;
private static Context logoutContext;

@Override
protected void onCreate(Bundle savedInstanceState) {
  super.onCreate(savedInstanceState);
  setContentView(R.layout.activity_main);
  
  restartTimer();
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



