#_C#_ 预处理指令（前置处理器指示词）

</br>
</br>

*	**#region**

	代码折叠的作用。
	
	```c sharp
	
	#region MyClass definition  
	public class MyClass   
	{  
	    static void Main()   
	    {  
	    }  
	}  
	#endregion  
	
	```
	######_备注_：
	*	`#region`区块必须以**#endregion**指示词结束。
	*	`#region`区块不能与**#if**区块重叠。但是`#region`区块可以巢状出现在`#if`区块中， 而`#if`区块可以巢状出现在`#region`区块中。

	
* **#pragma**

	会为编译器提供特殊指示，以供此编译器所在位置的档案编译使用。编译器必须要支持此指示。不能使用`#pragma`建立自定义前置处理指示。
	

* **#pragma warning**

	可以启用或停用特定警告。
	
	```c sharp
	#pragma warning disable warning-list  
	#pragma warning restore warning-list  

	```
	
	>`warning-list`
	>以逗号分隔的警告编号清单。输入编号即可，不需要带有"CS"前置词。
	
	示例：
	
	```c sharp
	#pragma warning disable 414, 3021 
	#pragma warning restore 3021 
	```

	
