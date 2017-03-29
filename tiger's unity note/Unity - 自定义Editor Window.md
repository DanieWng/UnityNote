#Unity - 自定义Editor Window

</br>

#####想要定制一个editor window需要准备三个步骤：

1.	创建一个继承自EditorWindow的派生类
2. 	编写能够触发显示它的代码
3. 自定义实现在GUI绘制你需要的工具


##Derive From EditorWindow

>In order to make your Editor Window, your script must be stored inside a folder called “**Editor**”. Make a class in this script that derives from **EditorWindow**. Then write your GUI controls in the inner **OnGUI** function.

##Showing the window

>In order to show the window on screen, make a menu item that displays it. This is done by creating a function which is activated by the MenuItem property.

>The default behavior in Unity is to recycle windows (so selecting the menu item again would show existing windows. This is done by using the function EditorWindow.GetWindow Like this:

>This will create a standard, dockable editor window that saves its position between invocations, can be used in custom layouts, etc. To have more control over what gets created, you can use GetWindowWithRect

##Implementing Your Window’s GUI

>The actual contents of the window are rendered by implementing the OnGUI function. You can use the same UnityGUI classes you use for your ingame GUI (GUI and GUILayout). In addition we provide some additional GUI controls, located in the editor-only classes EditorGUI and EditorGUILayout. These classes add to the controls already available in the normal classes, so you can mix and match at will.

>For more info, take a look at the example and documentation on the EditorWindow page. <https://docs.unity3d.com/ScriptReference/EditorWindow.html>


更多参考：<http://blog.theknightsofunity.com/custom-unity-editor-window/>


##EditorGUILayout 界面布局

**类函数：**
	
Function      | Description
------------- | -------------
LabelField |
Toggle |
TextField |
TextArea |
SelectableLabel |
PasswordField |
FloatField |
IntField |
Slider |
IntSlider |
MinMaxSlider |
Popup |
EnumPopup |
IntPopup |
TagField |
LayerField |
ObjectField |
Vector2Field |
Vector3Field |
Vector4Field |
RectField |
BoundsField |
ColorField |
CurveField |
InspectorTitleBar |
Foldout |
PrefixLabel |
Space |
BeginToggleGroup |
EndToggleGroup |
BeginHorizontal |
EndHorizontal |
BeginVertical |
EndVertical |
BeginScrollView |
EndScrollView |
PropertyField |

><http://www.ceeger.com/Script/EditorGUILayout/EditorGUILayout.html>


##EditorGUI 层级布局

```
EditorGUI.indentLevel += 1;
EditorGUI.indentLevel -= 1;
```

##GUIStyle & GUILayoutOption

>GUIStyleで見た目を変えよう <http://caitsithware.com/wordpress/archives/1391>