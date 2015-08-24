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

		private SparseArray<View> mViews;

		private View mConvertView;

		private int mPosition;

		public SuperViewHolder(Context context, ViewGroup parent, int layoutId, int position) {
			mPosition = position;
			mViews = new SparseArray<>();
			mConvertView = LayoutInflater.from(context).inflate(layoutId, parent, false);
			mConvertView.setTag(this);
		}

		public static SuperViewHolder make(Context context, int layoutId, View convertView, ViewGroup parent, int position) {
			if (convertView == null) {
				return new SuperViewHolder(context, parent, layoutId, position);
			} else {
				SuperViewHolder superViewHolder = (SuperViewHolder) convertView.getTag();
				superViewHolder.mPosition = position;
				return superViewHolder;
			}
		}

		public <T extends View> T getView(int viewId) {
			View view = mViews.get(viewId);
			if (view == null) {
				view = mConvertView.findViewById(viewId);
				mViews.put(viewId, view);
			}
			return (T) view;
		}

		public int getPosition() {
			return mPosition;
		}

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
            SuperViewHolder superViewHolder = SuperViewHolder.make(mContext, R.layout.item_calendar, convertView, parent, position);
            ((TextView)superViewHolder.getView(R.id.calendar_time)).setText(mDatas.get(position));
            return superViewHolder.getConvertView();
        }

    }

>

---

>

### 补充问题

>

	// 焦点抢占
	关于在ListView的Item里增加(例CheckBox对象)造成Item无法点击问题

	此问题是因为CheckBox对象抢占了Item的焦点，造成Item无法点击的

	只需要在将CheckBox对象android:focusable属性值设置为false即可

>

	// 复用导致内容错乱
	比如: 当Item中使用CheckBox时点击第一个则其它CheckBox也被选中了
		  这是因为使用的是同一个CheckBox对象的缘故 ...
	
	解决: 这样就需要记录CheckBox对象的状态来判断CheckBox是否被选中
		  可以通过position来作为ID，来记录CheckBox状态是True或False

>
