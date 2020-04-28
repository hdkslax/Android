# Activity 传递数据

## 1. 页面自动跳转

这里使用Handler，代码如下：

```java
// 定义全局变量 mHandler
Handler mHandler = new Handler();

// 在onCreate中，为mHandler赋值
mHandler.postDelayed(new Runnable()) {
    @Override 
    public void run() {
        // 跳转到目标activity
        Intent intent = new Intent(SplashActivity.this, TargetActivity.class);
        StartActivity(intent);
    }
}, 1000); // 1000 = 1s

```



## 2. 传递数据

利用Intent中的putExtra()和getXXXExtra()方法传递***基本数据类型***

```java
// 将数据传出去
mHandler.postDelayed(new Runnable()) {
    // 首先获取要传的值
    TextView textView = findViewById(R.id.title_text_view);
    final String title = textView.getText().toString // getText() returns a CharSequence type of data
    
    @Override 
    public void run() {
        // 跳转到目标activity
        Intent intent = new Intent(SplashActivity.this, TargetActivity.class);
        // putExtra("A",B)中，AB为键值对，第一个参数为键名，第二个参数为键对应的值。
        intent.putExtra('title', title); // 内部类，title要变成final，意为不可在内部更改
        StartActivity(intent);
    }
}, 1000); // 1000 = 1s

// targetActivity中获取数据

Intent intent = getIntent();
if(intent != null) { // 预防intent为空
    // getXXXXXExtra()方法.需要使用对应类型的方法，参数为键名
    intent.getStringExtra("title"); 
    
    // 比如传过来的String作为现在activity的action bar
    setTitle(title);
}
```



利用Intent中的putExtra()和getXXXExtra()方法传递***对象***

```java
// 第一步：将对象的类(class)中添加 
class Course implements Serializable{} //意为序列化

// 第二步：将对象传出去
mHandler.postDelayed(new Runnable()) {
    @Override 
    public void run() {
        Course course = new Course(line); // line中包含创建一个Course的所有信息
        // 跳转到目标activity
        Intent intent = new Intent(SplashActivity.this, TargetActivity.class);
        // putExtra("A",B)中，AB为键值对，第一个参数为键名，第二个参数为键对应的值。
        intent.putExtra("course",course); // 内部类，title要变成final，意为不可在内部更改
        StartActivity(intent);
    }
}, 1000); // 1000 = 1s

// 第三步：获取传来的对象
Intent intent = getIntent();
if(intent != null){
    // getSerializableExtra()方法.需要使用对应类型的方法，参数为键名
    Course course = (Course) intent.getSerializableExtra("course"); // 引号中的键名最好提取成常量
    // 比如传过来的String作为现在activity的action bar
    setTitle(course.getTitle());
}

```

## 3. 允许传回数据

```java
// 传输
mHandler.postDelayed(new Runnable()) {
    @Override 
    public void run() {
        Course course = new Course(line); // line中包含创建一个Course的所有信息
        // 跳转到目标activity
        Intent intent = new Intent(SplashActivity.this, TargetActivity.class);
        // putExtra("A",B)中，AB为键值对，第一个参数为键名，第二个参数为键对应的值。
        intent.putExtra("course",course); // 内部类，title要变成final，意为不可在内部更改
        // StartActivityForResult(Intent intent, int requestCode, Bundle options)
        StartActivityForResult(intent, 9999); // 第三个参数可不写
    }
}, 1000); // 1000 = 1s

// 接受(例如点击按钮之后传回值到之前的activity)
ImageButton addBtn = findViewById(R.id.addBtn);
        addBtn.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent();
                intent.putExtra("title", "I'm back");
                // void setResult(int resultCode, Intent data) 第二个参数可不写
                setResult(1234, intent);
                finish();
            }
        });
// 第三步：在传输数据的activity中新增 onActivityResult()
//field中定义一个TAG
private static final String TAG = SplashActivity.class.getSimpleName();
@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    super.onActivityResult(requestCode, resultCode, data);
    Log.i(TAG, "requestCode: " + requestCode + "resultCode:" + resultCode);
    // 通过该方法，可从多个activity中传回信息
    if(requestCode == 9999 && resultCode == 1234){
        if(data != null) {
            String title = data.getStringExtra("title");
        }
    }
    
}
```

