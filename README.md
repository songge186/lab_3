# Lab_3_UIComponent
# 实验三：UI组件

# 一、实验内容

1、利用SimpleAdapter实现如下效果

<image wide="300" height="400" src="https://github.com/jinrongrong815/img_folder/blob/master/Lab_3_1q.png">

2、创建如图所示的自定义对话框

<image wide="250" height="200" src="https://github.com/jinrongrong815/img_folder/blob/master/Lab_3_2q.png">

3、使用XML定义菜单

<image wide="300" height="400" src="https://github.com/jinrongrong815/img_folder/blob/master/Lab_3_3q.png">

4、创建上下文操作模式的上下文菜单

<image wide="300" height="400" src="https://github.com/jinrongrong815/img_folder/blob/master/Lab_3_4q.png">

# 二、关键代码
1、

public class SimpleAdapterActivity extends AppCompatActivity {

    private ListView listView;
    //private List<Entity> list;
    public int mCurrentItem=0;
    private String[] names = {"Lion", "Tiger", "Monkey", "Dog", "Cat", "Elephant"};
    private int[] images = {R.drawable.lion, R.drawable.tiger, R.drawable.monkey, R.drawable.dog, R.drawable.cat, R.drawable.elephant};

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.simpleadapter_layout);
        //初始化ListView控件
        listView = findViewById(R.id.listView);
        final MyBaseAdapator myBaseAdapator = new MyBaseAdapator();
        listView.setAdapter(myBaseAdapator);
        listView.setOnItemClickListener(new OnItemClickListener() {
            @Override
            public void onItemClick(AdapterView<?> parent, View view,
                                    int position, long id) {

                myBaseAdapator.notifyDataSetChanged();
                Toast.makeText(SimpleAdapterActivity.this,
                        names[position], Toast.LENGTH_LONG)
                        .show();
                mCurrentItem = position;
                myBaseAdapator.notifyDataSetChanged();

            }
        });

    }
    class MyBaseAdapator extends BaseAdapter{
        private LayoutInflater inflater;

        private int currentItem = -1;
        private boolean isClick=false;
        private int mSelect;   //选中项

        public void setCurrentItem(int currentItem){
            this.currentItem = currentItem;
        }
        @Override
        public int getCount(){
            return names.length;
        }
        @Override
        public Object getItem(int position){
            return names[position];
        }
        @Override
        public long getItemId(int position){
            return position;
        }
        @Override
        public View getView(int position, View convertView, ViewGroup parent){
             ViewHolder holder; 

            if (convertView == null) {
                convertView = LayoutInflater.from(getApplication()).inflate(R.layout.item_layout, parent, false);
                holder = new ViewHolder();
                holder.mTextView = convertView.findViewById(R.id.item_tv);
                holder.imageView = convertView.findViewById(R.id.item_img);
                convertView.setTag(holder);
            } else {
                holder = (ViewHolder) convertView.getTag();
            }
            holder.mTextView.setText(names[position]);
            holder.imageView.setBackgroundResource(images[position]);

            if(mCurrentItem == position){
                holder.mTextView.setEnabled(true);

            } else {
                listView.setBackgroundColor(Color.TRANSPARENT);
            }
            return convertView;
        }

        public void setClick(boolean click){
            this.isClick=click;
        }

        class ViewHolder {
            TextView mTextView;
            ImageView imageView;
        }
    }
}

2、

public class DialogActivity extends AppCompatActivity {
    
    @Override
    protected void onCreate(Bundle savedInstanceState) {

        super.onCreate(savedInstanceState);
        requestWindowFeature(Window.FEATURE_NO_TITLE);
        setContentView(R.layout.alertdialog);
        // 创建对话框构建器
        AlertDialog.Builder builder = new AlertDialog.Builder(this);
        // 获取布局
        View view2 = View.inflate(DialogActivity.this, R.layout.alertdialog, null);
        // 获取布局中的控件
        final EditText username = (EditText) view2.findViewById(R.id.username);
        final EditText password = (EditText) view2.findViewById(R.id.password);
        final Button btn_sign = (Button) view2.findViewById(R.id.btn_sign);
        final Button btn_cancel = (Button) view2.findViewById(R.id.btn_cancel);
        // 设置参数
        builder.setTitle("Android App").setIcon(R.drawable.ic_launcher_background)
                .setView(view2);
        // 创建对话框
        final AlertDialog alertDialog = builder.create();
        btn_sign.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                String uname = username.getText().toString().trim();
                String psd = password.getText().toString().trim();
                    Toast.makeText(DialogActivity.this, "登录成功", Toast.LENGTH_SHORT).show();
                alertDialog.dismiss();// 对话框消失
            }
        });
        btn_cancel.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                alertDialog.dismiss();// 对话框消失
            }
        });
        alertDialog.show();
    }
}

3、

public class XMLMenuActivity extends AppCompatActivity {

    private TextView textView;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.xmlmenu_layout);
        textView = (TextView) findViewById(R.id.editText);
        // 为文本框注册上下文菜单
        registerForContextMenu(textView);
    }

    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        MenuInflater inflater = new MenuInflater(this);
        //装填R.Menu.my_menu菜单，并添加到menu中
        inflater.inflate(R.menu.menu_main,menu);
        return super.onCreateOptionsMenu(menu);
    }
    //创建上下文菜单时触发该方法
    @Override
    public void onCreateContextMenu(ContextMenu menu, View v, ContextMenu.ContextMenuInfo menuInfo) {
       
    }
    //上下文菜单中菜单项被单击时，触发该方法

    @Override
    public boolean onContextItemSelected(MenuItem item) {
        
    }
    //菜单项被单击后的回调方法

    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        if (item.isCheckable()){
            //勾选菜单项
            item.setCheckable(true);
        }
        //switch 判断单击哪个菜单项，并有针对性的做出响应
        switch (item.getItemId()){
            case R.id.font_10:
                textView.setTextSize(10 * 2);
                break;
            case R.id.font_16:
                textView.setTextSize(16 * 2);
                break;
            case R.id.font_20:
                textView.setTextSize(20 * 2);
                break;
            case R.id.red_font:
                textView.setTextColor(Color.RED);
                break;
            case R.id.black_font:
                textView.setTextColor(Color.BLACK);
                break;
            case R.id.plain_item:
                Toast.makeText(XMLMenuActivity.this, "普通菜单项", Toast.LENGTH_SHORT).show();
                break;
        }
        return true;
    }

}

4、

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.contextmenu_layout);

        mListView = (ListView) findViewById(R.id.listView);
        mAdapter = new MultyAdapter();
        mCheckedPositions = new ArrayList<Integer>();

        mAnimateDismissAdapter = new AnimateDismissAdapter<ContextMenuActivity.Model>(
                mAdapter, new MyDismissCallBack());
        mAnimateDismissAdapter.setAbsListView(mListView);
        mListView.setAdapter(mAnimateDismissAdapter);
        mListView.setChoiceMode(ListView.CHOICE_MODE_MULTIPLE_MODAL);
        mListView.setMultiChoiceModeListener(new MultiChoiceModeListener() {

            @Override
            public boolean onPrepareActionMode(ActionMode mode, Menu menu) {
                return false;
            }

            @Override
            public void onDestroyActionMode(ActionMode mode) {

                // 在退出ActionMode的时候调用，如果处于删除状态，就删除选中的数据，
                // 否则，重置所有选中的状态
                if (!isInDeleteMode) {
                    for (Model model : mAdapter.models) {
                        model.setChecked(false);
                    }
                    mCheckedPositions.clear();
                }
                isInActionMode = false;
            }

            @Override
            public boolean onCreateActionMode(ActionMode mode, Menu menu) {
                // 在进入ActionMode的时候调用
                MenuInflater inflater = mode.getMenuInflater();
                inflater.inflate(R.menu.menu_delete, menu);
                mode.setTitle("Delete");
                isInActionMode = true;
                isInDeleteMode = false;
                return true;
            }

            @Override
            public boolean onActionItemClicked(ActionMode mode, MenuItem item) {

                // 当listview中的item被点击的时候调用
                if (item.getItemId() == R.id.action_delete) {
                    mAnimateDismissAdapter.animateDismiss(mCheckedPositions);
                    isInDeleteMode = true;
                    mode.finish();
                    return true;
                }
                return false;
            }

            @Override
            public void onItemCheckedStateChanged(ActionMode mode,
                                                  int position, long id, boolean checked) {

                // 当item的选中状态被选中的时候调用
                mAdapter.models.get(position).setChecked(checked);
                mAdapter.notifyDataSetChanged();
                mode.setSubtitle(mListView.getCheckedItemCount()
                        + " item selected");

                if (mCheckedPositions.contains(position) && !checked) {
                    mCheckedPositions.remove(Integer.valueOf(position));
                } else {
                    mCheckedPositions.add(position);
                }

            }
        });
    }
    
    @Override
    public View getView(int position, View convertView, ViewGroup parent) {
            ViewHolder viewHolder;
            Model model = mAdapter.models.get(position);
            if (convertView == null) {
                convertView = getLayoutInflater().inflate(
                        R.layout.contextmenu_item_layout, parent, false);

                viewHolder = new ViewHolder();

                viewHolder.tv = (TextView) convertView.findViewById(R.id.item_tv);
                viewHolder.chb = (CheckBox) convertView.findViewById(R.id.chb);
                convertView.setTag(viewHolder);
            } else {
                viewHolder = (ViewHolder) convertView.getTag();
            }
            viewHolder.tv.setText(model.getTitle());
            viewHolder.chb.setChecked(model.isChecked());
            viewHolder.chb.setVisibility(isInActionMode ? View.VISIBLE
                    : View.GONE);
            return convertView;
     }
     
     private class MyDismissCallBack implements OnDismissCallback {
        @Override
        public void onDismiss(AbsListView arg0, int[] arg1) {
            mCheckedPositions.clear();
            Iterator<Model> iterator = mAdapter.models.iterator();
            while (iterator.hasNext()) {
                if (iterator.next().isChecked()) {
                    // 删除选中的元素
                    iterator.remove();
                }
            }
            mAdapter.notifyDataSetChanged();
        }
    }
       
# 三、实验结果截图

1、

<image wide="300" height="350" src="https://github.com/jinrongrong815/img_folder/blob/master/Lab_3_1a.jpg">

2、

<image wide="300" height="350" src="https://github.com/jinrongrong815/img_folder/blob/master/Lab_3_2a.jpg">

3、

<image wide="300" height="350" src="https://github.com/jinrongrong815/img_folder/blob/master/Lab_3_3a1.jpg">

4、

<image wide="300" height="350" src="https://github.com/jinrongrong815/img_folder/blob/master/lab_3_4a.jpg">

