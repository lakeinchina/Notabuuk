# A
The ultimate test of activity
 
standard和singleTop都不会启动新的task
taskAffinity属性不对standard和singleTop模式有任何影响,即使你指定了其他值也不会创建新task

一个Act的taskAffinity实际为task最顶层的Act

standard：
不管启动什么Act都创建新的实例
TaskId和启动者相同
taskAffinity如果不指定就为包名

singleTop:FLAG_ACTIVITY_SINGLE_TOP
如果栈顶为该<A>的实例那么就newIntent，否则创建新的

singleTask:
根据taskAffinity查找有无task
  如果没有：创建一个task，然后创建新的Act
  如果有：查找该task是否存在该Act
    如果有：把它上边的Act弹出，然后调用onNewIntent
    如果没有：创建Act并入栈
指定taskAffinity可以跨app

singleInstance:
全系统唯一，占用一个task栈，这个栈里只能有自己

taskAffinity的意思是Act<优先>属于哪个任务，而不是<强制>属于哪个任务

android:allowTaskReparenting:
是否允许Act改变task
就是说，一个activity1原来属于task1，但是如果task2启动起来的话，activity1可能不再属于task1了，转而投奔task2去了。 当然前提条件是allowTaskReparenting，还有affinity设置

android:alwaysRetainTaskState:
是有Act保持状态
这个属性用来标记应用的task是否保持原来的状态，“true”表示总是保持，“false”表示不能够保证，默认为“false”。此属性只对task的根Activity起作用，其他的Activity都会被忽略。 默认情况下，如果一个应用在后台呆的太久例如30分钟，用户从主选单再次选择该应用时，系统就会对该应用的task进行清理，除了根Activity，其他Activity都会被清除出栈，但是如果在根Activity中设置了此属性之后，用户再次启动应用时，仍然可以看到上一次操作的界面。 这个属性对于一些应用非常有用，例如Browser应用程序，有很多状态，比如打开很多的tab，用户不想丢失这些状态，使用这个属性就极为恰当。

android:clearTaskOnLaunch
如果某个Activity的clearTaskOnLaunch设置为true。当该Activity位于某个task的栈底时，如果你离开当前的task而转到别的task；那么，该task中除了该Activity之外的其它Activity都会被删除！
这个属性用于设定在从主屏中重启任务时，处理根节点的Activity以外，任务中的其他所有的Activity是否要被删除。如果设置为true，那么任务根节点的Activity之上的所有Activity都要被清除，如果设置了false，就不会被清除。默认设置时false。这个属性只对启动新任务（或根Activity）的那些Activity有意义，任务中其他所有的Activity都会被忽略。

android:finishOnTaskLaunch
每当用户再次启动任务时（在主屏幕上选择该任务），已存在的 Activity 实例是否应该关闭
如果某个Activity的finishOnTaskLaunch设置位true。只要你一离开这个task栈, 则系统会马上清除这个Activity, 不管这个Activity在堆栈的任何位置。
这个属性用于设定在从主屏中重启任务时，处理根节点的Activity以外，任务中的其他所有的Activity是否要被删除。如果设置为true，那么任务根节点的Activity之上的所有Activity都要被清除，如果设置了false，就不会被清除。默认设置时false。这个属性只对启动新任务（或根Activity）的那些Activity有意义，任务中其他所有的Activity都会被忽略。

android:excludeFromRecents
Activity 是否排除在用户最近访问应用程序的列表（“recent apps”）之外 。 也就是说，如果本 Activity 是新任务中的根 Activity，则本属性确定了该任务是否不出现在最近应用程序列表中。

android:exported
Activity 是否能被其他应用程序的组件启动 —“true”表示可以，“false”表示不能。 如果设为“false”，那么 Activity 仅能被本应用程序或用户ID相同的应用程序的组件启动。

android:noHistory
当用户离开且屏幕上已看不到 Activity 时，是否要从栈中清除并结束（调用其 finish() 方法）它 — “true”表示需要结束，“false”表示不要结束。 默认值是“false”。

android:parentActivityName
当用户点击 ActionBar 的 Up 按钮时，系统将读取本属性以确定应该打开的 Activity 。 利用本属性，系统还可以通过 TaskStackBuilder 生成 Activity 的 back 栈。

android:stateNotNeeded
未经保存状态， Activity 是否可被杀死并能够成功重启 —“true”表示可以不管之前的状态而被重启， “false” 表示需要保存之前的状态。 默认值是“false”。
通常，在为了腾出资源而临时关闭 Activity 之前，将会调用 onSaveInstanceState() 方法。该方法把当前 Activity 的状态存储到一个 Bundle 对象中，重新启动 Activity 时该 Bundle 对象将会被传给 onCreate() 。如果本属性设为“true”，则onSaveInstanceState() 可能就不会被调用，传入onCreate() 的将为 null 而非 Bundle — 就像第一次启动 Activity 一样。
本属性设为“true”将确保 Activity 能未经保存状态而被重启。 比如，显示主屏幕的 Activity 就利用本属性来保证由于某种原因崩溃时不会被清除。

FLAG_ACTIVITY_NEW_TASK:
如果当前根taskAffinity和要启动的Act不同，就启动一个新的task

FLAG_ACTIVITY_NO_HISTORY:
不把它保存到history堆栈，一旦用户离开了这个activity，这个activity将会结束，这个属性可以通过在AndroidManifest的activity标签中使用noHistory设置


FLAG_ACTIVITY_MULTIPLE_TASK
不建议使用此标记，除非你自己实现了应用程序的启动器。结合FLAG_ACTIVITY_NEW_TASK这个标记，即使要启动的activity已经存在一个task在运行，也会新启动一个task来运行要启动的activity

FLAG_ACTIVITY_CLEAR_TOP：
如果栈里以及存在Act，弹出Act后面的，重建Act
加上FLAG_ACTIVITY_SINGLE_TOP不重建，调onNewIntent


FLAG_ACTIVITY_REORDER_TO_FRONT:
从栈里找以及存在的Act放到栈顶

FLAG_ACTIVITY_FORWARD_RESULT：
A->B->C，A要求Result，B设置此flag启动C，那么C会把result给A

FLAG_ACTIVITY_EXCLUDE_FROM_RECENTS：
不出现在最近应用列表

FLAG_ACTIVITY_RESET_TASK_IF_NEEDED:
一个task从后台回前台的时候，如果栈顶Act有次标志，那么销毁这个Act

FLAG_ACTIVITY_LAUNCHED_FROM_HISTORY：
通常系统设，表示这个Act是从最近应用列表启动的

FLAG_ACTIVITY_NO_USER_ACTION:
设了这个，旧Act在暂停的时候不会调onUserLeaveHint

FLAG_ACTIVITY_REORDER_TO_FRONT：
设了这个会把栈中的目标Act放到栈顶，如果用了FLAG_ACTIVITY_CLEAR_TOP,这个标记会被忽略

FLAG_ACTIVITY_NO_ANIMATION：
这次Activity切换不会用切换动画


FLAG_ACTIVITY_CLEAR_TASK：
必须和FLAG_ACTIVITY_NEW_TASK同时使用，表示启动以及存在的task的时候会把栈清空

FLAG_ACTIVITY_TASK_ON_HOME：
把新启动的task置于home任务之上，也就是说这个任务结束就回到home


# java跑js
```java
ScriptEngine engine;
engine = new ScriptEngineManager().getEngineByName("rhino");
String runjs = HttpUtil.getRequest("http://10.0.10.179/run.js");
Object o = engine.eval(runjs);
Log.e("laketest", "eval=" + o);
if (engine instanceof Invocable) {
    Invocable in = (Invocable) engine;
    double res = (Double) in.invokeFunction("main");
    Log.e("laketest", "res=" + res);
}
```
```javascript
println("This is hello from test.js");
var factor;
function main(){
	func1();
	return 1;
}

function func1(){

	var imp =new JavaImporter(Packages.lake.me.javascripttest);
	with(imp){
		var j = new JavaUtil();
		j.getDouble();
	}
}
```
