---
title: Android之万能适配器Adapter的使用
date: '2015-08-24'
description:
categories:

tags:android

---

>

### 万能适配器

>

万能适配器共有两个部分：SuperViewHolder 和 SuperBaseAdapter<T> 

>

### 万能的ViewHolder

>

	public class SuperViewHolder {

		// 以View的ID为KEY存储View对象
		private SparseArray<View> mViews;

		// 全局ConvertView
		private View mConvertView;

		// 构造SuperViewHolder
		public SuperViewHolder(Context context, ViewGroup parent, int layoutId) {
			mViews = new SparseArray<>();
			mConvertView = LayoutInflater.from(context).inflate(layoutId, parent, false);
			mConvertView.setTag(this);
		}

		// 创建一个SuperViewHolder
		public static SuperViewHolder make(Context context, int layoutId, View convertView, ViewGroup parent) {
			if (convertView == null) {
				return new SuperViewHolder(context, parent, layoutId);
			} else {
				SuperViewHolder superViewHolder = (SuperViewHolder) convertView.getTag();
				return superViewHolder;
			}
		}
	
		// 获取View
		public <T extends View> T getView(int viewId) {
			View view = mViews.get(viewId);
			if (view == null) {
				view = mConvertView.findViewById(viewId);
				mViews.put(viewId, view);
			}
			return (T) view;
		}

		// 获取ConvertView
		public View getConvertView() {
			return mConvertView;
		}

	}

>

---

>

### 万能的SuperBaseAdapter<T>

>

	public abstract class SuperBaseAdapter<T> extends BaseAdapter {

		protected Context mContext;

		protected List<T> mDatas;

		public SuperBaseAdapter(Context context, List<T> datas) {
			mContext = context;
			mDatas = datas;
		}

		@Override
		public int getCount() {
			return mDatas.size();
		}

		@Override
		public Object getItem(int position) {
			return mDatas.get(position);
		}

		@Override
		public long getItemId(int position) {
			return position;
		}

		@Override
		public abstract View getView(int position, View convertView, ViewGroup parent);

	}

>

---

>

### 结合使用 SuperViewHolder 和 SuperBaseAdapter<T>

>

    class CalendarBaseAdapter extends SuperBaseAdapter<String> {

        public CalendarBaseAdapter(Context context, List<String> datas) {
            super(context, datas);
        }

        @Override
        public View getView(int position, View convertView, ViewGroup parent) {
            SuperViewHolder superViewHolder = SuperViewHolder.make(mContext, R.layout.item_calendar, convertView, parent);
            ((TextView)superViewHolder.getView(R.id.calendar_time)).setText(mDatas.get(position));
            return superViewHolder.getConvertView();
        }

    }

>


