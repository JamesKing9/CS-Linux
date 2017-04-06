# <第一行代码-Android>

## 1.1.1 Android 系统架构
Android 大致可以分为四层架构，五块区域。
1. Linux 内核层
>Android 系统是基于 Linux 2.6 内核的，这一层为 Android 设备的各种硬件提供了底层的驱动，如显示驱动、音频驱动、照相机驱动、蓝牙驱动、Wi-Fi 驱动、电源管理等。
2. 系统运行库层
>这一层通过一些 C/C++库来为 Android 系统提供了主要的特性支持。 如 SQLite 库提供了数据库的支持，OpenGL|ES 库提供了 3D 绘图的支持，Webkit 库提供了浏览器内核的支持等。
>同样在这一层还有 Android 运行时库，它主要提供了一些核心库，能够允许开发者使用 Java 语言来编写 Android 应用。另外 Android 运行时库中还包含了 Dalvik 虚拟机，它使得每一个 Android 应用都能运行在独立的进程当中，并且拥有一个自己的 Dalvik 虚拟机实例。相较于 Java 虚拟机，Dalvik 是专门为移动设备定制的，它针对手机内存、CPU 性能有限等情况做了优化处理。
3. 应用框架层
>这一层主要提供了构建应用程序时可能用到的各种 API，Android 自带的一些核心应用就是使用这些API完成的， 开发者也可以通过使用这些API来构建自己的应用程序。
4. 应用层
>所有安装在手机上的应用程序都是属于这一层的，比如系统自带的联系人、短信等程序，或者是你从 Google Play 上下载的小游戏，当然还包括你自己开发的程序。

## 1.1.2 Android 已发布的版本

## 1.1.3 Android 应用开发特色
看一看，Android 系统到底提供了哪些东西，供我们可以开发出优秀的应用程序。
1. 四大组件
>Android系统四大组件分别是活动（Activity） 、服务（Service） 、广播接收器（BroadcastReceiver）和内容提供器（Content Provider） 。
>其中活动是所有 Android 应用程序的门面，凡是在应用中你看得到的东西，都是放在活动中的。
>而服务就比较低调了，你无法看到它，但它会一直在后台默默地运行，即使用户退出了应用，服务仍然是可以继续运行的。
>广播接收器可以允许你的应用接收来自各处的广播消息，比如电话、短信等，当然你的应用同样也可以向外发出广播消息。
>内容提供器则为应用程序之间共享数据提供了可能，比如你想要读取系统电话簿中的联系人，就需要通过内容提供器来实现。
2. 丰富的系统控件
>Android 系统为开发者提供了丰富的系统控件，使得我们可以很轻松地编写出漂亮
>的界面。当然如果你品味比较高，不满足于系统自带的控件效果，也完全可以定制属于自己的控件。
3. SQLite 数据库
>Android 系统还自带了这种轻量级、运算速度极快的嵌入式关系型数据库。它不仅支持标准的 SQL 语法，还可以通过 Android 封装好的 API 进行操作，让存储和读取数据变得非常方便。
4. 地理位置定位
>移动设备和 PC 相比起来，地理位置定位功能应该可以算是很大的一个亮点。现在
>的 Android 手机都内置有 GPS，走到哪儿都可以定位到自己的位置，发挥你的想象就可以做出创意十足的应用，如果再结合上功能强大的地图功能，LBS 这一领域潜力无限。
5. 强大的多媒体
>Android 系统还提供了丰富的多媒体服务，如音乐、视频、录音、拍照、闹铃等等，这一切你都可以在程序中通过代码进行控制，让你的应用变得更加丰富多彩。
6. 传感器
>Android 手机中都会内置多种传感器，如加速度传感器、方向传感器等，这也算是
>移动设备的一大特点。通过灵活地使用这些传感器，你可以做出很多在 PC 上根本无法实现的应用。


# 6.4 SQLite 数据库存储
>`SQLite` 是一款轻量级的关系型数据库，它的运算速度非常快，
>占用资源很少， 通常只需要几百 K 的内存就足够了， 因而特别适合在移动设备上使用。 SQLite不仅支持标准的 SQL 语法，还遵循了数据库的 `ACID` 事务，所以只要你以前使用过其他的关系型数据库，就可以很快地上手 SQLite。而 SQLite 又比一般的数据库要简单得多，它甚至不用设置用户名和密码就可以使用。Android 正是把这个功能极为强大的数据库嵌入到了系统当中，使得本地持久化的功能有了一次质的飞跃。
>前面我们所学的文件存储和 SharedPreferences存储毕竟只适用于去保存一些简单的数据和键值对，当需要存储大量复杂的关系型数据的时候，你就会发现以上两种存储方式很难应付得了。比如我们手机的短信程序中可能会有很多个会话，每个会话中又包含了很多条信息内容，并且大部分会话还可能各自对应了电话簿中的某个联系人。很难想象如何用文件或者SharedPreferences 来存储这些数据量大、结构性复杂的数据吧？但是使用数据库就可以做得到。

## 6.4.1 创建数据库
>Android 为了让我们能够更加方便地管理数据库，专门提供了一个SQLiteOpenHelper 帮助类， 借助这个类就可以非常简单地对数据库进行创建和升级。
> SQLiteOpenHelper 是一个抽象类，这意味着如果我们想要使用它的话，就需要创建一个自己的帮助类去继承它。SQLiteOpenHelper 中有两个抽象方法，分别是
>onCreate()和 onUpgrade()，我们必须在自己的帮助类里面重写这两个方法，然后分别在这两个方法中去实现创建、升级数据库的逻辑。SQLiteOpenHelper 中 还 有 两 个 非 常 重 要 的 实 例 方 法 ，getReadableDatabase() 和getWritableDatabase()。这两个方法都可以创建或打开一个现有的数据库（如果数据库已存在则直接打开，否则创建一个新的数据库） ，并返回一个可对数据库进行读写操作的对象。不同的是，当数据库不可写入的时候（如磁盘空间已满）getReadableDatabase()方法返回的对象将以只读的方式去打开数据库，而 getWritableDatabase()方法则将出现异常。
>SQLiteOpenHelper 中有两个构造方法可供重写，一般使用参数少一点的那个构造方法即可。这个构造方法中接收四个参数，第一个参数是 Context，必须要有它才能对数据库进行操作。第二个参数是数据库名，创建数据库时使用的就是这里指定的名
>称。第三个参数允许我们在查询数据的时候返回一个自定义的 Cursor，一般都是传入 null。第 四 个 参 数 表 示 当 前 数 据 库 的 版 本 号 ， 可 用 于 对 数 据 库 进 行 升 级 操 作 。 构 建 出SQLiteOpenHelper 的实例之后，再调用它的getReadableDatabase()或 getWritableDatabase()方
>法就能够创建数据库了，数据库文件会存放在/data/data/<package name>/databases/目录下。此时，重写的 onCreate()方法也会得到执行，所以通常会在这里去处理一些创建表的逻辑。

接下来还是让我们通过例子的方式来更加直观地体会 SQLiteOpenHelper 的用法吧，首先新建一个 DatabaseTest 项目。
这里我们希望创建一个名为 BookStore.db的数据库， 然后在这个数据库中新建一张 Book表，表中有 id（主键） 、作者、价格、页数和书名等列。创建数据库表当然还是需要用建表语句的，这里也是要考验一下你的 SQL 基本功了，Book 表的建表语句如下所示：
```java
create table Book (
id integer primary key autoincrement,
author text,
price real,
pages integer,
name text)
```
SQLite 不像其他的数据库拥有众多繁杂的数据类型， 它的数据类型很简单， integer 表示整型，real 表示浮点型，text 表示文本类型，blob 表示二进制类型。另外，上述建表语句中我们还使用了 primary key 将 id 列设为主键，并用 autoincrement 关键字表示 id 列是自增长的。
然后需要在代码中去执行这条 SQL 语句， 才能完成创建表的操作。 新建 MyDatabaseHelper类继承自 SQLiteOpenHelper，代码如下所示：
```java
public class MyDatabaseHelper extends SQLiteOpenHelper {
  public static final String CREATE_BOOK = "create table book ("
  + "id integer primary key autoincrement, "
  + "author text, "
  + "price real, "
  + "pages integer, "
  + "name text)";

  private Context mContext;
  public MyDatabaseHelper(Context context, String name, CursorFactory
  factory, int version) {
  super(context, name, factory, version);
  mContext = context;
  }
  @Override
  public void onCreate(SQLiteDatabase db) {
  db.execSQL(CREATE_BOOK); //执行建表语句
  Toast.makeText(mContext, "Your Table Create succeeded", Toast.LENGTH_SHORT).show();
  }
  @Override
  public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {
  }
}
```
现在修改 activity_main.xml 中的代码，如下所示：
```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
android:layout_width="match_parent"
android:layout_height="match_parent"
android:orientation="vertical" >
<Button
android:id="@+id/create_database"
android:layout_width="match_parent"
android:layout_height="wrap_content"
android:text="Create database"
/>
</LinearLayout>
```
布局文件很简单，就是加入了一个按钮，用于创建数据库。最后修改 MainActivity 中的代码，如下所示：
```java
public class MainActivity extends Activity {
  private MyDatabaseHelper dbHelper;
  
  @Override
  protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);
    dbHelper = new MyDatabaseHelper(this, "BookStore.db", null, 1);
    Button createDatabase = (Button) findViewById(R.id.create_database);
    createDatabase.setOnClickListener(new OnClickListener() {
      @Override
      public void onClick(View v) {
      dbHelper.getWritableDatabase();
      }
    });
  }
}
```
这里我们在 onCreate()方法中构建了一个 MyDatabaseHelper 对象，并且通过构造函数的参数将数据库名指定为 BookStore.db，版本号指定为 1，然后在 Create database 按钮的点击事件里调用了 getWritableDatabase()方法。这样当第一次点击 Createdatabase按钮时，就会检测到当前程序中并没有 BookStore.db 这个数据库，于是会创建该数据库并调用 MyDatabaseHelper中的 onCreate()方法，这样 Book 表也就得到了创建，然后会弹出一个 Toast 提示创建成功。再次点击 Create database 按钮时，会发现此时已经存在 BookStore.db 数据库了，因此不会再创建一次。

## 6.4.2 升级数据库
如果你足够细心， 一定会发现 MyDatabaseHelper中还有一个空方法呢！ 没错， onUpgrade()方法是用于对数据库进行升级的，它在整个数据库的管理工作当中起着非常重要的作用，可千万不能忽视它哟。
目前 DatabaseTest 项目中已经有一张 Book 表用于存放书的各种详细数据，如果我们想再添加一张 Category 表用于记录书籍的分类该怎么做呢？
比如 Category 表中有 id（主键） 、分类名和分类代码这几个列，那么建表语句就可以写成：
```java
create table Category (
id integer primary key autoincrement,
category_name text,
category_code integer)
```
接下来我们将这条建表语句添加到 MyDatabaseHelper 中，代码如下所示：
```java
public class MyDatabaseHelper extends SQLiteOpenHelper {
public static final String CREATE_BOOK = "create table Book ("
+ "id integer primary key autoincrement, "
+ "author text, "
+ "price real, "
+ "pages integer, "
+ "name text)";
public static final String CREATE_CATEGORY = "create table Category ("
+ "id integer primary key autoincrement, "
+ "category_name text, "
+ "category_code integer)";
+ 
private Context mContext;
public MyDatabaseHelper(Context context, String name,
CursorFactory factory, int version) {
super(context, name, factory, version);
mContext = context;
}
@Override
public void onCreate(SQLiteDatabase db) {
db.execSQL(CREATE_BOOK);
db.execSQL(CREATE_CATEGORY);
Toast.makeText(mContext, "Create succeeded", Toast.LENGTH_SHORT).
show();
}
@Override
public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {
}
}
```
看上去好像都挺对的吧， 现在我们重新运行一下程序， 并点击 Create database 按钮， 咦？竟然没有弹出创建成功的提示。当然，你也可以通过 adb 工具到数据库中再去检查一下，这样你会更加地确认，Category 表没有创建成功！
其实没有创建成功的原因不难思考，因为此时 BookStore.db 数据库已经存在了，之后不管我们怎样点击 Create database 按钮，MyDatabaseHelper 中的 onCreate()方法都不会再次执行，因此新添加的表也就无法得到创建了。
解决这个问题的办法也相当简单，只需要先将程序卸载掉，然后重新运行，这时
BookStore.db 数据库已经不存在了，如果再点击 Create database 按钮,MyDatabaseHelper 中的 onCreate()方法就会执行，这时 Category 表就可以创建成功了。
不过通过卸载程序的方式来新增一张表毫无疑问是很极端的做法，其实我们只需要巧妙地运用 SQLiteOpenHelper 的升级功能就可以很轻松地解决这个问题。 修改 MyDatabaseHelper中的代码，如下所示：
```java
public class MyDatabaseHelper extends SQLiteOpenHelper {
……
@Override
public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {
db.execSQL("drop table if exists Book");
db.execSQL("drop table if exists Category");
onCreate(db);
}
}
```
可以看到，我们在 onUpgrade()方法中执行了两条 DROP 语句，如果发现数据库中已经存在 Book 表或 Category 表了，就将这两张表删除掉，然后再调用 onCreate()方法去重新创建。这里先将已经存在的表删除掉，是因为如果在创建表时发现这张表已经存在了，就会直接报错。
接下来的问题就是如何让 onUpgrade()方法能够执行了，还记得 SQLiteOpenHelper 的构造方法里接收的第四个参数吗？它表示当前数据库的版本号，之前我们传入的是 1，现在只要传入一个比 1 大的数， 就可以让 onUpgrade()方法得到执行了。 修改 MainActivity 中的代码，如下所示：
```java
public class MainActivity extends Activity {
private MyDatabaseHelper dbHelper;
@Override
protected void onCreate(Bundle savedInstanceState) {
super.onCreate(savedInstanceState);
setContentView(R.layout.activity_main);
dbHelper = new MyDatabaseHelper(this, "BookStore.db", null, 2);
Button createDatabase = (Button) findViewById(R.id.create_database);
createDatabase.setOnClickListener(new OnClickListener() {
@Override
public void onClick(View v) {
dbHelper.getWritableDatabase();
}
});
}
}
```
这里将数据库版本号指定为 2，表示我们对数据库进行升级了。现在重新运行程序，并点击 Create database 按钮，这时就会再次弹出创建成功的提示。为了验证一下 Category 表是不是已经创建成功了，我们在 adb shell 中打开 BookStore.db 数据库，然后键入.table 命令，结果如图 6.19 所示。

## 6.4.3 添加数据
现在你已经掌握了创建和升级数据库的方法，接下来就该学习一下如何对表中的数据进行操作了。其实我们可以对数据进行的操作也就无非四种，即 CRUD。其中 C 代表添加（Create） ，R 代表查询（Retrieve） ，U 代表更新（Update） ，D 代表删除（Delete） 。每一种操作又各自对应了一种 SQL 命令，如果你比较熟悉 SQL 语言的话，一定会知道添加数据时使用 insert，查询数据时使用 select，更新数据时使用 update，删除数据时使用 delete。但是开发者的水平总会是参差不齐的，未必每一个人都能非常熟悉地使用 SQL 语言，因此 Android也是提供了一系列的辅助性方法，使得在 Android 中即使不去编写 SQL 语句，也能轻松完成
所有的 CRUD 操作。
前面我们已经知道，调用 SQLiteOpenHelper的 getReadableDatabase()或 getWritableDatabase()方法是可以用于创建和升级数据库的， 不仅如此， 这两个方法还都会返回一个SQLiteDatabase对象，借助这个对象就可以对数据进行 CRUD 操作了。
首先学习一下如何向数据库的表中添加数据吧。
SQLiteDatabase 中提供了一个 insert()方法，这个方法就是专门用于添加数据的。它接收三个参数，第一个参数是表名，我们希望向哪张表里添加数据，这里就传入该表的名字。第二个参数用于在未指定添加数据的情况下给某些可为空的列自动赋值 NULL，一般我们用不到这个功能，直接传入 null 即可。第三个参数是一个 ContentValues 对象，它提供了一系列的 put()方法重载，用于向 ContentValues 中添加数据，只需要将表中的每个列名以及相应的待添加数据传入即可。
介绍完了基本用法，接下来还是让我们通过例子的方式来亲身体验一下如何添加数据吧。修改 activity_main.xml 中的代码，如下所示：
```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
android:layout_width="match_parent"
android:layout_height="match_parent"
android:orientation="vertical" >
……
<Button
android:id="@+id/add_data"
android:layout_width="match_parent"
android:layout_height="wrap_content"
android:text="Add data"
/>
</LinearLayout>
```
可以看到，我们在布局文件中又新增了一个按钮，稍后就会在这个按钮的点击事件里编写添加数据的逻辑。接着修改 MainActivity 中的代码，如下所示：
```java
public class MainActivity extends Activity {
private MyDatabaseHelper dbHelper;
@Override
protected void onCreate(Bundle savedInstanceState) {
super.onCreate(savedInstanceState);
setContentView(R.layout.activity_main);
dbHelper = new MyDatabaseHelper(this, "BookStore.db", null, 2);
……
Button addData = (Button) findViewById(R.id.add_data);
addData.setOnClickListener(new OnClickListener() {
@Override
public void onClick(View v) {
SQLiteDatabase db = dbHelper.getWritableDatabase();
ContentValues values = new ContentValues();
// 开始组装第一条数据
values.put("name", "The Da Vinci Code");
values.put("author", "Dan Brown");
values.put("pages", 454);
values.put("price", 16.96);
db.insert("Book", null, values); // 插入第一条数据
values.clear();
// 开始组装第二条数据
values.put("name", "The Lost Symbol");
values.put("author", "Dan Brown");
values.put("pages", 510);
values.put("price", 19.95);
db.insert("Book", null, values); // 插入第二条数据
}
});
}
}
```
在添加数据按钮的点击事件里面，我们先获取到了 SQLiteDatabase 对象，然后使用
ContentValues 来对要添加的数据进行组装。 如果你比较细心的话应该会发现， 这里只对 Book表里其中四列的数据进行了组装，id 那一列没并没给它赋值。这是因为在前面创建表的时候我们就将 id 列设置为`自增长`了，它的值会在入库的时候自动生成，所以不需要手动给它赋值了。 接下来调用了 insert()方法将数据添加到表当中， 注意这里我们实际上添加了两条数据，上述代码中使用 ContentValues 分别组装了两次不同的内容，并调用了两次 insert()方法。
好了，现在可以重新运行一下程序了，界面如图 6.21 所示。

点击一下 Add data 按钮，此时两条数据应该都已经添加成功了，不过为了证实一下，我们还是打开 BookStore.db 数据库瞧一瞧。输入 SQL 查询语句 select * from Book，结果如图6.22 所示。





# 第 10 章 看看精彩的世界，使用网络技术

# 10.2 使用 HTTP 协议访问网络
如果真的说要去深入分析 HTTP 协议，可能需要花费整整一本书的篇幅。这里我当然不会这么干，因为毕竟你是跟着我学习 Android 开发的，而不是网站开发。对于 HTTP 协议，你只需要稍微了解一些就足够了，它的工作原理特别的简单，就是客户端向服务器发出一条HTTP 请求，服务器收到请求之后会返回一些数据给客户端，然后客户端再对这些数据进行解析和处理就可以了。是不是非常简单？一个浏览器的基本工作原理也就是如此了。比如说上一节中使用到的 WebView 控件，其实也就是我们向百度的服务器发起了一条 HTTP 请求，接着服务器分析出我们想要访问的是百度的首页，于是会把该网页的 HTML 代码进行返回，然后 WebView再调用手机浏览器的内核对返回的 HTML代码进行解析， 最终将页面展示出来。
简单来说，WebView 已经在后台帮我们处理好了发送 HTTP 请求、接收服务响应、解析返回数据，以及最终的页面展示这几步工作，不过由于它封装得实在是太好了，反而使得我们不能那么直观地看出 HTTP 协议到底是如何工作的。因此，接下来就让我们通过手动发送HTTP 请求的方式，来更加深入地理解一下这个过程。

## 10.2.1 使用 HttpURLConnection
在 Android 上发送 HTTP 请求的方式一般有两种，HttpURLConnection 和 HttpClient，本小节我们先来学习一下 HttpURLConnection 的用法。
首先需要获取到 HttpURLConnection 的实例，一般只需 new 出一个 URL 对象，并传入目标的网络地址，然后调用一下 openConnection()方法即可，如下所示：
```java
URL url = new URL("http://www.baidu.com");
HttpURLConnection connection = (HttpURLConnection) url.openConnection();
```
得到了 HttpURLConnection 的实例之后，我们可以设置一下 HTTP 请求所使用的方法。常用的方法主要有两个，GET 和 POST。GET 表示希望从服务器那里获取数据，而 POST 则表示希望提交数据给服务器。写法如下：
`connection.setRequestMethod("GET");`
接下来就可以进行一些自由的定制了，比如设置连接超时、读取超时的毫秒数，以及服务器希望得到的一些消息头等。这部分内容根据自己的实际情况进行编写，示例写法如下：
```java
connection.setConnectTimeout(8000);
connection.setReadTimeout(8000);
```
之后再调用 getInputStream()方法就可以获取到服务器返回的输入流了， 剩下的任务就是对输入流进行读取，如下所示：
`InputStream in = connection.getInputStream();`
最后可以调用 disconnect()方法将这个 HTTP 连接关闭掉，如下所示：
`connection.disconnect();`

# 12.3 加速度传感器

## 12.3.2 模仿微信摇一摇
```
用加速度传感器来模仿一下微信的摇一摇功能。
其实主体逻辑也非常简单，只需要检测手机在 X 轴、Y 轴和 Z 轴上的加速度，当达到了预定值的时候就可以认为用户摇动了手机，从而触发摇一摇的逻辑。那么现在问题在于，这个预定值应该设定为多少呢？由于重力加速度的存在，即使手机在静止的情况下，某一个轴上的加速度也有可能达到9.8m/s2，因此这个预定值必定是要大于 9.8m/s2的，这里我们就设定为 15m/s2吧。
```
新建一个 AccelerometerSensorTest 项目，然后修改 MainActivity 中的代码，如下所示：
```java
public class MainActivity extends Activity {
  private SensorManager sensorManager;
  
  @Override
  protected void onCreate(Bundle saveInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);
    sensorManager = (SensorManager)getSystemService(Context.SENSOR_SERVICE);
    Sensor sensor = sensorManager.getDefaultSensor(Sensor.TYPE_ACCELEROMETER);
    sensorManager.registerListener(listener, sensor, SensorManager.SENSOR_DELAY_NORMAL);
  }
  
  @Override
  protected void onDestroy() {
    super.onDestroy();
    if(sensorManager != null) {
      sensorManager.unregisterListener(listener);
    }
  }
  
  private SensorEventListener listener = new SensorEvnetListener() {
    @Override
    public void onSensorChanged(SensorEvent event) {
      float xValue = Math.abs(event.values[0]);
      float yValue = Math.abs(event.values[1]);
      float zValue = Math.abs(event.values[2]);
      if (xValue > 15 || yValue > 15 || zValue >15) {
        Toast.makeText(MainActivity.this, "摇一摇", Toast.LENGTH_SHORT).show();
      }
    }
    
    @Override
    public void onAccuracyChanged(Sensor sensor, int accuracy)
  }
}
```

