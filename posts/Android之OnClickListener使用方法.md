---
title: Android之OnClickListener使用方法
date: '2015-03-24'
description:
categories:

tags:android

---

>

### OnClickListener

>

	// ...	
        TextView text1 = (TextView)findViewById(R.id.text1);
        text1.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                // ...
            }
        });

---

>

### OnClickListener

>

	// ...
        TextView text1 = (TextView)findViewById(R.id.text1);
        OnItemClickListener onClickListener = new OnItemClickListener();
        text1.setOnClickListener(onClickListener);

	// ...
	class OnItemClickListener implements View.OnClickListener {

		@Override
		public void onClick(View v) {
		    // TODO Auto-generated method stub
		    if(v.getId() == R.id.text1){
			showDetail(1);
		    }
		    else if(v.getId() == R.id.text2){
			showDetail(2);
		    }
		    else if(v.getId() == R.id.text3){
			showDetail(3);
		    }
		    else if(v.getId() == R.id.text4){
			showDetail(4);
		    }
		}
	}


