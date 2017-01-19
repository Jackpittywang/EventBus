# EventBus使用

开源地址：[https://github.com/greenrobot/EventBus](https://github.com/greenrobot/EventBus)

* 爱生活,爱学习,更爱做代码的搬运工,分类查找更方便请下载黑马助手app


![黑马助手.png](http://upload-images.jianshu.io/upload_images/4037105-f777f1214328dcc4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## 使用步骤

### 1. 在Module的build.gradle添加依赖

	compile 'org.greenrobot:eventbus:3.0.0'


### 2. 创建事件
	public class MyEvent {
	    public String msg;
	    public MyEvent(String msg) {
	        this.msg = msg;
	    }
	}

### 3. 注册和反注册EventBus
    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        //注册事件总线
        EventBus.getDefault().register(this);
    }


    @Override
    protected void onDestroy() {
        super.onDestroy();
        //反注册事件总线
        EventBus.getDefault().unregister(this);
    }


### 4. 监听事件
	
	/**
     * POSTING线程模型：在哪个线程发布事件，就在哪个线程执行onPostingEvent方法
     */
    @Subscribe(threadMode = ThreadMode.POSTING)
    public void onPostingEvent(MyEvent event) {
        Log.d(TAG, "onPostingEvent: " + Thread.currentThread().getName());
    }


    /**
     * MAIN线程模型：不管是哪个线程发布事件，都在主线程执行onMainEvent方法
     */
    @Subscribe(threadMode = ThreadMode.MAIN)
    public void onMainEvent(MyEvent event) {
        Log.d(TAG, "onMainEvent: " + Thread.currentThread().getName());
    }

    /**
     * BACKGROUND线程模型：事件如果是在子线程发布，onBackgroundEvent方法就在该子线程执行，事件如果是在
     * 主线程中发布，onBackgroundEvent方法就在EventBus内部的线程池中执行
     */
    @Subscribe(threadMode = ThreadMode.BACKGROUND)
    public void onBackgroundEvent(MyEvent event) {
        Log.d(TAG, "onBackgroundEvent: " + Thread.currentThread().getName());
    }

    /**
     * ASYNC线程模型：不管事件在哪个线程发布，onAsyncEvent方法都在EventBus内部的线程池中执行
     */
    @Subscribe(threadMode = ThreadMode.ASYNC)
    public void onAsyncEvent(MyEvent event) {
        Log.d(TAG, "onAsyncEvent: " + Thread.currentThread().getName());
    }

### 5. 发布事件
    /**
     * 在主线程中发布事件
     * @param view
     */
    public void onPublishEventOnMainThread(View view) {
        MyEvent event = new MyEvent("msg from publisher main thread");
        EventBus.getDefault().post(event);
    }

    /**
     * 在子线程中发送事件
     * @param view
     */
    public void onPublishEventOnBGThread(View view) {
        new Thread(new Runnable() {
            @Override
            public void run() {
                MyEvent event = new MyEvent("msg from publisher bg thread");
                EventBus.getDefault().post(event);
            }
        }).start();
    }


* 详细的使用方法在DEMO里面都演示啦,如果你觉得这个库还不错,请赏我一颗star吧~~~

* 欢迎关注微信公众号

![](http://upload-images.jianshu.io/upload_images/4037105-8f737b5104dd0b5d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


