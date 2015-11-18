---
title: Android之Broadcast发送广播
date: '2015-11-18'
description:
categories:

tags:android

---

>

### Android之Broadcast发送广播

>

	// 第一步

    // 创建一个BroadcastReceiver消息广播类
    public class MessageRecevier extends BroadcastReceiver {

        @Override
        public void onReceive(Context context, Intent intent) {
            // 收到广播，通知更新 ( 这里可以是 Object)
            String obj = intent.getStringExtra("obj");
            int what = intent.getIntExtra("what", 0);
			// 把收到的广播再发出去
            Message m = new Message();
            m.obj = obj;
            m.what = what;
            message.sendMessage(m);
        }

    }

	// 第二步

	// 广播绑定
	public final static String MESSAGE_RECEVIER_ACTION = "com.danoo.caraide";
	MessageRecevier mRecevier = new MessageRecevier();
	registerReceiver(mRecevier, new IntentFilter(MESSAGE_RECEVIER_ACTION));

	// 第三步
	
	// 发送广播
	Intent order = new Intent(MESSAGE_RECEVIER_ACTION);
	order.putExtra("obj", "order");
	order.putExtra("what", msg.getOrder());
	context.sendBroadcast(order);

	// 第四步

	// 收工
	@Override
    protected void onDestroy() {
        super.onDestroy();
        // 取消广播接收器
        unregisterReceiver(mRecevier);
    }

>

