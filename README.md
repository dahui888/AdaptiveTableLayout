# 自适应表格布局 [![Awesome](https://cdn.rawgit.com/sindresorhus/awesome/d7305f38d29fed78fa85652e3a63e154dd8e8829/media/badge.svg)](https://github.com/sindresorhus/awesome) 


## 欢迎 Cleveroad 推出适用于 Android 的新 CSV 库 AdaptiveTableLayout

请注意我们的新库，它使读取、编辑和写入 CSV 文件成为可能。如果您使用基于 Android 的设备，您可以轻松使用我们的库来实现上述所有操作。此外，您将能够更改行和列，通过链接显示图片，并对齐所需的数据。它肯定会帮助您更快地处理任务并提高您的输出。AdaptiveTableLayout 库可供您使用。

![Demo image](/images/demo.gif)

#### Take a look at the animation of <strong><a target="_blank" href="https://www.youtube.com/watch?v=YTwpEPIlhuE">AdaptiveTableLayout for Android on YouTube</a></strong> in HD quality. For using this library in a valuable way, you can find our CSV Editor app on the <a target="_blank"  href="https://play.google.com/store/apps/details?id=com.cleveroad.tablelayout">Google Play Store</a> or on <a target="_blank"  href="https://appetize.io/app/wgacjavwr57fec241bq802gzcg?device=nexus5&scale=75&orientation=portrait&osVersion=7.0">Appetize</a>.
[![Awesome](/images/youtube.png)](https://www.youtube.com/watch?v=YTwpEPIlhuE)[![Awesome](/images/google-play.png)](https://play.google.com/store/apps/details?id=com.cleveroad.tablelayout)[![Awesome](/images/appertize.png)](https://appetize.io/app/wgacjavwr57fec241bq802gzcg?device=nexus5&scale=75&orientation=portrait&osVersion=7.0)

该库的主要目标是在处理 CSV 文件的过程中应用其所有功能。此外，它会让你比其他人更具竞争优势。

[![Awesome](/images/logo-footer.png)](https://www.cleveroad.com/?utm_source=github&utm_medium=label&utm_campaign=contacts)
<br/>
## 导入和使用
### 导入
通过 gradle : 
```groovy
dependencies {
    implementation "com.cleveroad:adaptivetablelayout:1.2.1"
}
```
### 特征 ###
库由三部分组成：
- AdaptiveTableLayout（View）
- LinkedAdaptiveTableAdapter（Adapter）
- ViewHolderImpl (ViewHolder)

### 用法 ###
#### AdaptiveTableLayout ####
```XML
  <com.cleveroad.adaptivetablelayout.AdaptiveTableLayout
        android:id="@+id/tableLayout"
        android:layout_width="match_parent"
        android:layout_height="match_parent"      
        app:cellMargin="1dp"
        app:fixedHeaders="true"
        app:solidRowHeaders="true"
        app:dragAndDropEnabled="true"/>
```
|  属性名称	 | 描述 |
|---|---|
| cellMargin  | 单元格边距 |
| fixedHeaders  | 固定标题模式。如果启用，标题将始终显示在角落。|
| solidRowHeaders  | 实心行标题模式。如果启用，行标题将随着拖动行改变其位置。|
| dragAndDropEnabled | 拖放模式。如果启用，长按行或列标题后，列或行将通过拖动更改其位置。|

```groovy
//返回固定标题模式
boolean isHeaderFixed();

//返回实体行标题模式
boolean isSolidRowHeader();

//返回拖放模式
boolean isDragAndDropEnabled();

//如果布局方向是 RightToLeft 则返回 true 
boolean isRTL();

//设置固定标题模式
void setHeaderFixed ( boolean headerFixed )

//设置实体行标题模式
void setSolidRowHeader ( boolean solidRowHeader )
 1.2.0 
//设置拖放模式
void setDragAndDrow ( boolean enabled )

/**
 * 使用不可变数据设置适配器。
* 创建带有布局行、列和数据行、列之间链接的包装器。
* 在拖放事件中只更改链接但不更改适配器中的数据。
*/ void setAdapter ( @Nullable AdaptiveTableAdapter adapter) 
   

/**
 * 使用 MUTABLE 数据设置适配器。
* 需要实现切换行列的方法。    
* 请勿用于大数据！！
*/ void setAdapter ( @Nullable DataAdaptiveTableLayoutAdapter adapter) 
   

//通知任何已注册的观察者数据集已更改。
void notifyDataSetChanged ()

//通知任何已注册的观察者该项目已更改。
void notifyItemChanged ( int rowIndex , int columnIndex )

//通知任何注册的观察者，rowIndex 的行已经改变。
void notifyRowChanged ( int rowIndex )

//通知所有已注册的观察者 columnIndex 的列已更改。
void notifyColumnChanged ( int columnIndex )
```
#### 适配器 ####
您可以使用适配器接口：AdaptiveTableAdapter 和 DataAdaptiveTableLayoutAdapter。但为了简化使用，库包含基本适配器： <b>BaseDataAdaptiveTableLayoutAdapter。</b> 和 <b>LinkedAdaptiveTableAdapter</b>.

<b>BaseDataAdaptiveTableLayoutAdapter</b> -处理光数据的简单适配器。警告！在每行/列切换时，原始数据将被更改。

<b>LinkedAdaptiveTableAdapter</b> - 适用于大量数据的适配器。警告！这种类型的适配器不会更改原始数据。它包含带有更改项和链接的矩阵。要获取更改的数据，您需要使用 AdaptiveTableLayout.getLinkedAdapterRowsMo​​difications() 和 AdaptiveTableLayout.getLinkedAdapterColumnsModifications()。不要忘记检查 AdaptiveTableLayout.isSolidRowHeader() 标志。如果为 false，则需要忽略在每一行中切换第一个 elemet。

<b>对于这两个适配器，在将适配器设置为 AdaptiveTableLayout 之前，您需要知道所有行/列的宽度、高度和行/列计数。</b>
#### Fragment/Activity 使用 ####
```groovy
mTableLayout = (AdaptiveTableLayout) view.findViewById(R.id.tableLayout);
...
mTableAdapter = new SampleLinkedTableAdapter(getContext(), mCsvFileDataSource);
mTableAdapter.setOnItemClickListener(...);
mTableAdapter.setOnItemLongClickListener(...);
mTableLayout.setAdapter(mTableAdapter);
...
mTableLayout.setHeaderFixed(true);
mTableLayout.setSolidRowHeader(true);
mTableAdapter.notifyDataSetChanged();
```
#### XML 用法 ####
```groovy
 <com.cleveroad.adaptivetablelayout.AdaptiveTableLayout
        android:id="@+id/tableLayout"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:layout_below="@+id/toolbar"
        app:cellMargin="1dp"
        app:fixedHeaders="true"
        app:solidRowHeaders="true"
        app:dragAndDropEnabled="true"/>
```
#### Adapter使用 ####
<a href="sample/src/main/java/com/cleveroad/sample/adapter/SampleLinkedTableAdapter.java"> Adapter 示例 </a>

