---
title: Android之ZXing从相册扫描二维码
date: '2015-04-14'
description:
categories:

tags:android

---

>

	### ZXing

>

	// 监听按钮触发选择相册图片
        photoCameraButton = (Button) this.findViewById(R.id.btn_photo_camera);
        photoCameraButton.getBackground().setAlpha(50);
        photoCameraButton.setOnClickListener(new OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent innerIntent = new Intent();
                innerIntent.setAction(Intent.ACTION_GET_CONTENT);
                innerIntent.setType("image/*");
                Intent wrapperIntent = Intent.createChooser(innerIntent, "选择二维码图片");
                CaptureActivity.this
                        .startActivityForResult(wrapperIntent, 1001);
            }
        });

>

	  // 监听返回
	  @Override
	    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
		super.onActivityResult(requestCode, resultCode, data);
		if (resultCode == RESULT_OK) {
		    switch (requestCode) {
			case 1001:

			    // 图片
			    Bitmap bitmap = null;

			    String[] proj = {MediaStore.Images.Media.DATA};

			    // 获取选中图片的路径
			    Cursor cursor = getContentResolver().query(data.getData(), proj, null, null, null);
			    if (cursor.moveToFirst()) {
				int column_index = cursor.getColumnIndexOrThrow(MediaStore.Images.Media.DATA);
				try {
				    bitmap = RGBLuminanceSource.loadBitmap(cursor.getString(column_index));
				} catch (FileNotFoundException e) {
				    Log.e("Err", e.toString());
				    e.printStackTrace();
				}
			    }

			    Hashtable<EncodeHintType, String> hints = new Hashtable<EncodeHintType, String>();
			    hints.put(EncodeHintType.CHARACTER_SET, "utf-8");
			    RGBLuminanceSource source = new RGBLuminanceSource(bitmap);
			    BinaryBitmap bitmap1 = new BinaryBitmap(new HybridBinarizer(source));
			    QRCodeReader reader2 = new QRCodeReader();
			    Result result = null;
			    try {
				try {
				    result = reader2.decode(bitmap1, hints);
				} catch (ChecksumException e) {
				    // TODO Auto-generated catch block
				    Toast.makeText(getApplicationContext(), "失败", Toast.LENGTH_SHORT).show();
				} catch (FormatException e) {
				    // TODO Auto-generated catch block
				    Toast.makeText(getApplicationContext(), "失败", Toast.LENGTH_SHORT).show();
				}
				// 成功
				if (result != null) {
				    // System.out.println("Result:" + result.getText());
				    Intent resultIntent = new Intent();
				    Bundle bundle = new Bundle();
				    bundle.putString("result", result.getText());
				    resultIntent.putExtras(bundle);
				    this.setResult(RESULT_OK, resultIntent);
				    CaptureActivity.this.finish();
				} else {
				    Toast.makeText(getApplicationContext(), "失败", Toast.LENGTH_SHORT).show();
				}
			    } catch (NotFoundException e) {
				// TODO Auto-generated catch block
				Toast.makeText(getApplicationContext(), "失败", Toast.LENGTH_SHORT).show();
			    }
			    break;
		    }
		}
	    }


>

	// 除了jar包，这里还要用到一个类就是
	/*
	 * Copyright 2009 ZXing authors
	 *
	 * Licensed under the Apache License, Version 2.0 (the "License");
	 * you may not use this file except in compliance with the License.
	 * You may obtain a copy of the License at
	 *
	 *      http://www.apache.org/licenses/LICENSE-2.0
	 *
	 * Unless required by applicable law or agreed to in writing, software
	 * distributed under the License is distributed on an "AS IS" BASIS,
	 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
	 * See the License for the specific language governing permissions and
	 * limitations under the License.
	 */
	 
	package ;
	 
	import java.io.FileNotFoundException;
	 
	import android.graphics.Bitmap;
	import android.graphics.BitmapFactory;
	 
	import com.google.zxing.LuminanceSource;
	 
	/**
	 * This class is used to help decode images from files which arrive as RGB data
	 * from Android bitmaps. It does not support cropping or rotation.
	 * 
	 * @author dswitkin@google.com (Daniel Switkin)
	 */
	public final class RGBLuminanceSource extends LuminanceSource {
	    private final byte[] luminances;
	 
	    public RGBLuminanceSource(String path) throws FileNotFoundException {
		this(loadBitmap(path));
	    }
	 
	    public RGBLuminanceSource(Bitmap bitmap) {
		super(bitmap.getWidth(), bitmap.getHeight());
		int width = bitmap.getWidth();
		int height = bitmap.getHeight();
		int[] pixels = new int[width * height];
		bitmap.getPixels(pixels, 0, width, 0, 0, width, height);
		// In order to measure pure decoding speed, we convert the entire image
		// to a greyscale array
		// up front, which is the same as the Y channel of the
		// YUVLuminanceSource in the real app.
		luminances = new byte[width * height];
		for (int y = 0; y < height; y++) {
		    int offset = y * width;
		    for (int x = 0; x < width; x++) {
			int pixel = pixels[offset + x];
			int r = (pixel >> 16) & 0xff;
			int g = (pixel >> 8) & 0xff;
			int b = pixel & 0xff;
			if (r == g && g == b) {
			    // Image is already greyscale, so pick any channel.
			    luminances[offset + x] = (byte) r;
			} else {
			    // Calculate luminance cheaply, favoring green.
			    luminances[offset + x] = (byte) ((r + g + g + b) >> 2);
			}
		    }
		}
	    }
	 
	    @Override
	    public byte[] getRow(int y, byte[] row) {
		if (y < 0 || y >= getHeight()) {
		    throw new IllegalArgumentException(
			    "Requested row is outside the image: " + y);
		}
		int width = getWidth();
		if (row == null || row.length < width) {
		    row = new byte[width];
		}
		System.arraycopy(luminances, y * width, row, 0, width);
		return row;
	    }
	 
	    // Since this class does not support cropping, the underlying byte array
	    // already contains
	    // exactly what the caller is asking for, so give it to them without a copy.
	    @Override
	    public byte[] getMatrix() {
		return luminances;
	    }
	 
	    private static Bitmap loadBitmap(String path) throws FileNotFoundException {
		Bitmap bitmap = BitmapFactory.decodeFile(path);
		if (bitmap == null) {
		    throw new FileNotFoundException("Couldn't open " + path);
		}
		return bitmap;
	    }
	}

>

	但是可能由于版本问题，QRCodeReader.decode()方法的参数有点变化，可以参考
	http://blog.csdn.net/cgwcgw_/article/details/10817121

	Map<DecodeHintType, String> map = new HashMap<DecodeHintType, String>();  
		map.put(DecodeHintType.CHARACTER_SET, "utf-8");

	HashMap<DecodeHintType, String> hints = new HashMap<DecodeHintType, String>();  
	hints.put(DecodeHintType.CHARACTER_SET, "utf-8");

	Hashtable<EncodeHintType, String> hints = new Hashtable<EncodeHintType, String>();
	hints.put(EncodeHintType.CHARACTER_SET, "utf-8");



