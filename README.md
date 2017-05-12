# NotePadMay
initial
## 一.功能介绍：
### 1.时间戳</br>
### 2.便签搜索（根据标题模糊搜索）</br>
### 3.UI美化</br>
### 4.背景颜色更改(对话框弹出式)</br>
## 二.技术分析：
### 1.时间戳分析：</br>
因为数据库中已有时间字段，所以只需要格式化时间存入即可</br>
NoteEditor.updateNote()函数中修改</br>
        SimpleDateFormat df = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");//设置日期格式</br>
        values.put(NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE, df.format(new Date()));
 ### 2.便签搜索
   private void addSearchView() 
   {
        //给listview添加头部(search)
        
        View v=View.inflate(this, R.layout.notelistheader,null);
        getListView().addHeaderView(v);
        //给搜索框添加搜索功能
        final EditText et_Search=(EditText)v.findViewById(R.id.et_search);
        et_Search.addTextChangedListener(new TextWatcherForSearch(){
            @Override
            public void onTextChanged(CharSequence charSequence, int i, int i1, int i2) {
                super.onTextChanged(charSequence, i, i1, i2);
                if (charSequence.length()!=0 && et_Search.getText().toString().length()!=0){
                    String str_Search = et_Search.getText().toString();
                    Cursor search_cursor = managedQuery(
                            getIntent().getData(),            // Use the default content URI for the provider.
                            PROJECTION,                       // Return the note ID and title for each note.
                            NotePad.Notes.COLUMN_NAME_TITLE+" like ?",     //模糊查询数据库
                            new String[]{"%"+str_Search+"%"}, //匹配字符串条件                            // No where clause, therefore no where column values.
                            NotePad.Notes.DEFAULT_SORT_ORDER  // Use the default sort order.
                    );
                    adapter.swapCursor(search_cursor);//用搜索结果的cursor刷新listview

                }else {
                    if (cursor!=null)//删除搜索框中的text后刷新listview
                    adapter.swapCursor(cursor);//刷新listview
                }
            }
        });
    }
 ### 3.改变背景颜色
    使用底部按钮弹出对话框
      private void setBackgroundTwo(){
        AlertDialog.Builder builder=new AlertDialog.Builder(NotesList.this);
        LayoutInflater inflater=getLayoutInflater();
        View view=inflater.inflate(R.layout.dialogcolor,null);
        LinearLayout linearLayout=(LinearLayout) view.findViewById(R.id.dialoglinear);
        final int[] colorArray={
                Color.parseColor("#707070"),//黑
                Color.parseColor("#fcf9a4"),//黄
                Color.parseColor("#abed65"),//绿
                Color.parseColor("#33a9e1"),//蓝
                Color.parseColor("#070707"),//黑
                Color.parseColor("#1cdaef"),//蓝绿
                Color.parseColor("#fa77ab"),//粉色
        };
        for (int i=0;i<7;i++){//动态创建imageview 用以显示不同颜色
            ImageView imageView=new ImageView(NotesList.this);
            imageView.setLayoutParams(new LinearLayout.LayoutParams(90,120));
            imageView.setBackgroundColor(colorArray[i]);
            final int finalI = i;
            imageView.setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View v) {
                    NoteAttribute.snoteBackground=colorArray[finalI];
                    getListView().setBackgroundColor(NoteAttribute.snoteBackground);
                    getListView().setBackgroundColor(colorArray[finalI]);
                    appbackground
                            .edit()
                            .putInt(NoteAttribute.NOTEBACKGROUND,colorArray[finalI])
                            .apply();//使用轻量级存储sharepreference异步将颜色存储在本地
                }
            });
            linearLayout.addView(imageView);
        }
        builder.setView(view).create().show();

    }
## 二.功能演示
### 1>基本功能
#### 1.notepad的主界面</br>
![image](https://github.com/Incredible-May/NotePadMay/blob/master/image/notepad%E4%B8%BB%E7%95%8C%E9%9D%A2.png)
</br>
#### 2.添加便签
![image](https://github.com/Incredible-May/NotePadMay/blob/master/image/%E6%B7%BB%E5%8A%A0%E4%BE%BF%E7%AD%BE.png)
#### 3.添加便签后的界面
![image](https://github.com/Incredible-May/NotePadMay/blob/master/image/%E6%B7%BB%E5%8A%A0%E5%90%8E%E7%9A%84%E7%95%8C%E9%9D%A2.png)
#### 4.点击搜索
![image](https://github.com/Incredible-May/NotePadMay/blob/master/image/%E7%82%B9%E5%87%BB%E6%90%9C%E7%B4%A2.png)
#### 5.搜索结果
![image](https://github.com/Incredible-May/NotePadMay/blob/master/image/%E6%90%9C%E7%B4%A2%E7%BB%93%E6%9E%9C.png)
### 2>扩展功能
#### 1.改变背景色
![image](https://github.com/Incredible-May/NotePadMay/blob/master/image/%E9%80%89%E6%8B%A9%E8%83%8C%E6%99%AF%E8%89%B2.png)
#### 2.改变后的界面
![image](https://github.com/Incredible-May/NotePadMay/blob/master/image/%E6%94%B9%E5%8F%98%E8%83%8C%E6%99%AF%E8%89%B2.png)
