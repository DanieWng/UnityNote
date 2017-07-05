#Unity - 添加Firebase

##Firebase Analytics

###LogEvent

1. 自动收集的事件
	
	>[자동으로 수집되는 이벤트](https://support.google.com/firebase/answer/6317485?hl=ko)

2. 内置Event
	
	>[文档](https://firebase.google.com/docs/reference/android/com/google/firebase/analytics/FirebaseAnalytics.Event)
	
3. 自定义

###安装

1. 使用官方的Firebase plugin package

####iOS

*	<h5>注意事项</h5>

	1. <del>下载GoogleService-Info.plist文件并放在`unity project/asset/`路径下任意位置， 编译**xcode**项目时，会自动拷贝到项目内
	2. <del>`Build`
	3. <del>在导出的**xcode**项目内有一个注意点就是虽然添加了**plugin**在**unity**内部，但是`iOS`仍然需要添加**framework**才行，编译xcode项目时，已经自动创建好了`Podfile`文件。 使用**cocoapods**命令即可
	4. <del>如果发现执行完`pod install`或者`pod update`命令后，没有出现`xcworkspace `后缀名的文件时，检查`Podfile`内是否包含了`:integrate_targets => false`这段定义。设置为**false**的话，**cocoapods**就不会自动生成`xcworkspace`</del>
	5. 编译可能会出现的错误记录:
	
		* <del>`diff: /../Podfile.lock: No such file or directory`
		
			<del>这是因为在`Build Phases`->`[CP]Check Pods Manifest.lock`内的脚本文件中，出现了未定义的路径
			
			<del>**解决方法**：
			
			<del>在`Build Settings`->`User-Defined`内添加:
			
			* <del>`PODS_ROOT`: `${SRCROOT}/Pods`
			* <del>`PODS_PODFILE_DIR_PATH`: `${SRCROOT}`
			
			> <del>[CocoaPodsで起きた問題と解決方法](http://www.kurisankaku.xyz/entry/2017/03/19/213440)
			
			> <del>[Cocopods安装使用和错误](http://www.jianshu.com/p/b5315bf42975)
		
		* <del>`duplicate symbol _OBJC_CLASS_$_PodsDummy_Pods_Unity_iPhone in:` 
	
			<del>这是`cocoapods`引起的符合重复问题，解决思路：
				
			* <del>删除项目的**Derived Data**, `File`->`Workspace Setting`->Find`Derived Data`	
			* <del>删除相关的文件夹，整体删除也可以，是不是会产生别的问题还没有试过

	#####测试后还是出现错误！！！ 暂时无解。 更换思路，`pod install`时不创建`xcworkspace`文件。 使用`xcodeproj`文件打开，并解决`_OBJC_CLASS_$_FIROptions" Error`。
	
	1. `Build Run` 
	2. `GoogleService-Info.plist` 已经拷贝到了`iOS`根目录下，但是有可能没有被添加到工程中，这个需要确认
	3.  意外的发现，在**Unity**执行`Build & Run`操作的话，编译成功了
		
	> ["_OBJC_CLASS_$_FIROptions" Error 解決策 ( Unity Build -> Xcode )](http://kojirohashida.hatenablog.com/entry/2017/01/18/124806)	

*	<h5>使用</h5>

	* `앱 버전` 不需要单独设置
	* `FirebaseAnalytics.SetCurrentScreen(screenName, screenClass)` 追踪场景
	* `Debug View`功能的开启／关闭
		1. **Xcode**下使用快捷键`shift` + `command` + `<`
		2. 在`Run`->`Arguments`->`Argument Passed On Launch`下添加命令
		3. `-FIRDebugEnabled` 或者  `-FIRDebugDisabled`
		[DebugView](https://firebase.google.com/docs/analytics/debugview)

	
####Android

* <h5>使用</h5>

	* **DebugView**
		1. install apk
		2. run common in terminal
			* **enable**: `adb shell setprop debug.firebase.analytics.app <package_name>` 
			* **disable**: `adb shell setprop debug.firebase.analytics.app .none.` 

	