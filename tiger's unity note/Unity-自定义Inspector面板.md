#Unity - 自定义Inspector面板

</br>
</br>

## 概念
*	代码文件和变量名的改变， 以大写字母为基准进行分割
* 	在面板中修改属性的值
	1.	在runtime运行模式下
	2. 	在开发模式下

*	将变量列在面板中或者不显示在面板中:
	1. public/private类型
	2. [SerializeField]/[System.NonSerialized]
	3. [System.Serializable]
	4. [HideInInspector]
	
## 编写自定义的Inspector

##### SplitAlphaWithUIImage.cs
``` c sharp
using UnityEngine;
using UnityEngine.UI;
using System.Collections;

public class SplitAlphaWithUIImage : Image
{

    [SerializeField]
    private Texture2D m_rgbTex2D;

    [SerializeField]
    private Texture2D m_alphaTex2D;
    
    
}
```

##### SplitAlphaWithUIImageEditor.cs
``` c sharp
using UnityEditor;
using UnityEngine;

[CustomEditor(typeof(SplitAlphaWithUIImage))]
public class SplitAlphaWithUIImageEditor : Editor
{
    private SerializedObject m_serializedObject;

    private SerializedProperty m_rbgTexture2D;
    private SerializedProperty m_alphaTexture2D;

    void OnEnable()
    {
        m_serializedObject = new SerializedObject(target);

        m_rbgTexture2D = m_serializedObject.FindProperty("m_rgbTex2D");
        m_alphaTexture2D = m_serializedObject.FindProperty("m_alphaTex2D");
    }

    public override void OnInspectorGUI()
    {
        m_serializedObject.Update();

        EditorGUILayout.PropertyField(m_rbgTexture2D);
        EditorGUILayout.PropertyField(m_alphaTexture2D);

        m_serializedObject.ApplyModifiedProperties();
    }

}
```

#####编写自定义Inspertor是基于自定义脚本的
1.	自定义一个脚本或类文件
2.	定义一个编辑这个类的Inspector的类，该类必须继承与Editor或EditorWindow，并存放在Editor目录下

#####解析自定义Editor类
*	头部加入 `using UnityEditor`
* 	`[CustomEditor(typeof(ClassName))]`
*  继承自**Editor**
*  序列化一个物体， 就是说脚本会附在一个object上，还有脚本的变量
*  在`OnEnable()`里面初始化object对象，抓取脚本里面的属性
*  再重写`OnInspectorGUI()`
*  更新脚本信息 `m_serializedObject.Update()`
*  使用`EditorGUILayout.PropertyField`暴露属性
*  `m_serializedObject.ApplyModifiedProperties()` 应用修改的属性

#####在Inpsector面板显示效果
![](https://github.com/DanieWng/UnityNote/blob/master/sceneshot/Jietu20170321-143233.jpg?raw=true)

#####那么当我们在面板上编辑属性时，如何与脚本进行互动呢？

##### SplitAlphaWithUIImage.cs

```c sharp
    private void UpdateTexture2D()
    {
        Material m = new Material(Shader.Find("Custom/SplitAlphaUIImageShader") as Shader);
        m.SetTexture(MAIN_TEX, m_RGBSource);
        m.SetTexture(ALPHA_TEX, m_AlphaSource);

        this.material = m;
    }

    private void CleanMaterial()
    {
        this.material = null;
        Resources.UnloadUnusedAssets();
    }

#if UNITY_EDITOR

    #pragma warning disable 0114
    public void OnValidate()
    {
        if(m_RGBSource && m_AlphaSource)
        {
            UpdateTexture2D();
        }
        else
        {
            CleanMaterial();
        }
    }
#endif
```
>`OnValidate()`方法在脚本属性发生改变时调用

_在编辑器加载属性改变image对象的演示视频：_ <https://www.youtube.com/watch?v=m7ImpuICOio>


