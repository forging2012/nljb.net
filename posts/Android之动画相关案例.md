---
title: Android之动画相关案例
date: '2016-02-18'
description:
categories:

tags:android

---

>

### 弹出与收缩 View Layout 

>

    // 动画显示 View
    private void showViewAnimation(final View v) {
        // 点击隐藏View
        v.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                hideViewAnimation(v);
            }
        });
        // AnimationSet 把多个动画组合成一个组合的机制
        AnimationSet animSet = new AnimationSet(true);
        // 缩放动画效果（从小到大）
        ScaleAnimation sa = new ScaleAnimation(
                // 动画起始时 X 坐标上的伸缩尺寸
                // View 宽 / View 父 宽
                // 宽 / 父宽 = 0 - 1 范围 宽 比例
                (float) v.getWidth() / ((View) v.getParent()).getWidth(),
                // 动画结束时 X 坐标上的伸缩尺寸
                1.0f,
                // 动画起始时 Y 坐标上的伸缩尺寸
                // View 高 / View 父 高
                // 高 / 父高 = 0 - 1 范围 高 比例
                (float) v.getHeight() / ((View) v.getParent()).getHeight(),
                // 动画结束时 Y 坐标上的伸缩尺寸
                1.0f,
                // 缩放起点 X 轴坐标，可以是数值、百分数、百分数p 三种样式，比如 50、50%
                // View 的 X 坐标 + View 宽 / 2
                // 从 X 坐标 开始缩放
                // 从 View 左上角开始
                v.getX(),// + v.getWidth() / 2,
                // 缩放起点 Y 轴坐标，可以是数值、百分数、百分数p 三种样式，比如 50、50%
                // View 的 Y 坐标 + View 高 / 2
                // 从 Y 坐标 开始缩放
                // 从 View 左上角开始
                v.getY());// + v.getHeight() / 2);
        // 设置动画持续时间
        sa.setDuration(1000);
        // 透明度渐变动画（从透明到不透明）
        AlphaAnimation aa = new AlphaAnimation(0.2f, 1);
        // 设置动画持续时间
        aa.setDuration(2000);
        // 并可设置组中动画的时序关系，如同时播放，顺序播放等
        animSet.addAnimation(sa);
        animSet.addAnimation(aa);
        // 开始执行动画
        v.startAnimation(animSet);
        // 显示 View
        v.setVisibility(View.VISIBLE);
    }

    // 动画隐藏 View
    private void hideViewAnimation(View v) {
        // AnimationSet 把多个动画组合成一个组合的机制
        AnimationSet animSet = new AnimationSet(true);
        // 缩放动画效果（从大到小）
        ScaleAnimation sa = new ScaleAnimation(
                // 从 X 坐标，最大开始
                1,
                // 宽 / 父宽 = 0 - 1 范围 宽 比例
                (float) v.getWidth() / ((View) v.getParent()).getWidth(),
                // 从 Y 坐标，最大开始
                1,
                // 高 / 父高 = 0 - 1 范围 高 比例
                (float) v.getHeight() / ((View) v.getParent()).getHeight(),
                // 从 View 右下角开始
                v.getX() + v.getWidth(),
                // 从 View 右下角开始
                v.getY() + v.getHeight());
        // 设置动画持续时间
        sa.setDuration(1000);
        // 透明度渐变动画（从不透明到透明）
        AlphaAnimation aa = new AlphaAnimation(1f, 0f);
        // 设置动画持续时间
        aa.setDuration(2000);
        // 并可设置组中动画的时序关系，如同时播放，顺序播放等
        animSet.addAnimation(sa);
        animSet.addAnimation(aa);
        // 开始执行动画
        v.startAnimation(animSet);
        // 隐藏 View
        v.setVisibility(View.GONE);
    }

>

---

>

