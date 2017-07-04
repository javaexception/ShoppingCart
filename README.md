# ShoppingCart
Android实现购物车页面及购物车效果(点击动画)

效果图如下：<br>

![alt text](https://github.com/javaexception/ShoppingCart/blob/master/app/src/main/res/raw/gouwuchegif.gif)<br>

思路：<br>
（1）思考每个条目中的数字的更新原理。<br>
（2）购物车的动画效果。<br>
（3）购物清单怎么显示(这个我暂时没有写，如果需要的话，可以在我的简书下给我留言)。<br>

1.因为进入页面，所有的商品个数都显示为零，所以我用 ArrayList<HashMap<String, Object>> data，把商品集合都附上零：<br>
```
        //下面把data都添加0，为了刚开始显示时，显示的是0
        for (int i = 0; i < list.size(); i++) {
            HashMap<String, Object> myhashmap = new HashMap<String, Object>();
            myhashmap.put("number", "" + 0);
            data.add(myhashmap);
        }
```
然后把data传入Adapter:<br>
```
     adapter = new MyAdapter(data);
```
当我们对商品进行增减时，我们可以通过hashmap来更改，如下是增加商品的部分代码:<br>
```
    b = Integer.parseInt((String) data.get(position).get(
                            "number"));
                    data.get(position).put("number", "" + (b + 1));
```

2.购物车动画效果:<br>
 首先获取点击时的XY坐标，并且设置动画图片:<br>
 
 ```
 // ball是个imageview
  startLocation = new int[2];// 一个整型数组，用来存储按钮的在屏幕的X、Y坐标
                    view.getLocationInWindow(startLocation);// 这是获取购买按钮的在屏幕的X、Y坐标（这也是动画开始的坐标）
                    ball = new ImageView(MainActivity.this);
                    ball.setImageResource(R.mipmap.sign);// 设置动画的图片我的是一个小球（R.mipmap.sign）
 ```
 然后是开始执行动画:<br>
 
 ```
     private void setAnim(final View v, int[] startLocation) {
        anim_mask_layout = null;
        anim_mask_layout = createAnimLayout(); //创建动画层
        anim_mask_layout.addView(v);//把动画小球添加到动画层
        final View view = addViewToAnimLayout(anim_mask_layout, v,
                startLocation);
        int[] endLocation = new int[2];// 存储动画结束位置的X、Y坐标
        re_zhongcai_tanchu.getLocationInWindow(endLocation);// re_zhongcai_tanchu是那个抛物线最后掉落的控件

        // 计算位移
        int endX = 0 - startLocation[0] + 40;// 动画位移的X坐标
        int endY = endLocation[1] - startLocation[1];// 动画位移的y坐标
        TranslateAnimation translateAnimationX = new TranslateAnimation(0,
                endX, 0, 0);
        translateAnimationX.setInterpolator(new LinearInterpolator());
        translateAnimationX.setRepeatCount(0);// 动画重复执行的次数
        translateAnimationX.setFillAfter(true);

        TranslateAnimation translateAnimationY = new TranslateAnimation(0, 0,
                0, endY);
        translateAnimationY.setInterpolator(new AccelerateInterpolator());
        translateAnimationY.setRepeatCount(0);// 动画重复执行的次数
        translateAnimationX.setFillAfter(true);

        final AnimationSet set = new AnimationSet(false);
        set.setFillAfter(false);
        set.addAnimation(translateAnimationY);
        set.addAnimation(translateAnimationX);
        set.setDuration(800);// 动画的执行时间
        view.startAnimation(set);
        // 动画监听事件
        set.setAnimationListener(new Animation.AnimationListener() {
            // 动画的开始
            @Override
            public void onAnimationStart(Animation animation) {
                v.setVisibility(View.VISIBLE);
                //    Log.e("动画","asdasdasdasd");
            }

            @Override
            public void onAnimationRepeat(Animation animation) {
                // TODO Auto-generated method stub
            }

            // 动画的结束
            @Override
            public void onAnimationEnd(Animation animation) {
                v.setVisibility(View.GONE);
                set.cancel();
                animation.cancel();
            }
        });

    }
 
 ```
 需要注意的是，当动画结束必须关闭动画:<br>
 
  ```
    v.setVisibility(View.GONE);
                set.cancel();
                animation.cancel();
  ```
  
  3.  购物车的弹出清单功能，我没有写，需要的话，可以去我的简书留言.<br>
  
  [我的简书地址](http://www.jianshu.com/u/2a55d6e39135)
  
  我的公众号如下:<br>
  
  ![alt text]( https://github.com/javaexception/ShoppingCart/blob/master/app/src/main/res/raw/qzs1.jpg)<br>
