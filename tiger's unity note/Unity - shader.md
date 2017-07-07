#Unity - shader

##Basic

* surface shader
* vertex and fragment shader
* fixed function shader

>一个render会需要一个material， 一个material对应着一个shader

**shader**的作用是把模型的网格数据进行处理的过程.

####共同点：
1. 都必须从唯一一个根shader开始
2. Properties参数部分， 作用和语法相同
3. 具体功能都在SubShader里（subshader又叫子shader，shader会自上而下运行第一个硬件能支持的subshader，主要作用是对不同硬件的支持）
4. 都可以打标签
5. 都可以Fallback(如果所有的subshader都不支持的情况下调用)
6. 都可以处理基本的功能，例如光照漫反射（diffuse）和镜面反射（specular）。

####不同点：
1. 固定渲染和顶点片段渲染在subshader下面还有`pass{}`,但是表面渲染由于已经将具体内容打包在光照模型上了，所以不能加`pass{}`,否则会报错。
2. 固定渲染的每行代码结尾没有`;`，但是顶点片段渲染和表面渲染必须以`;`结尾
3. 核心结构不同：

	**Fixed Function Shader**的核心是`Material{}` 以及 `SetTexture[_MainTex]{}`
	
	**Vertex and Fragment Shader**的核心是：
	
	```
	CGPROGRAM
	
	\#pragma vertex vert
	
	\#pragma fragment frag  
	  
	\#include "UnityCG.cginc" 
	
	ENDCG
	```
	
	**Surface Shader**的核心是：
	
	* 如果用的是unity自带的光照模型Lambert, 也不需要做顶点处理，那么只需要一个表面处理函数`surf`即可。

		```
		CGPROGRAM
		
		#pragma surface surf Lambert
		
		ENDCG
		```
		
	* 如果用的是自己写的光照模型IsyLightModel,并且使用了顶点处理函数`vert`

		```
		CGPROGRAM
		
		// surface 表面处理函数   光照模型函数      顶点处理:函数
		#pragma surface  surf         lsyLightModel      vertex:vert
		
		//执行顺序   顶点处理函数 -> 表面处理函数 -> 光照模型函数 ->颜色值
		
		ENDCG
		```
		
		
##Shader结构

#### 基本代码结构
```
Shader "名称"{
	
	// 属性
	Properties{
		
	}
	
	// 子着色器
	// 可以有多个子着色器，会根据硬件从上到下，选择当前硬件支持的代码进行运算
	SubShader{
		Pass{
		
		}
	}
	
	// 如果所有子着色器都满足不了，那么调用这个
	FallBack "Diffuse"

}
```

#### 属性

|语法|说明|
|:------------- | -------------:|
|名称("显示名称", Vector)=默认值|四维向量属性|
|名称("显示名称", Color)=默认值|颜色（0，1四维向量）|
|名称("显示名称", Float)=默认值|浮点数|
|名称("显示名称", Range(min,max))=默认值|随机浮点数|
|名称("显示名称", 2D)=默认值|2D纹理|
|名称("显示名称", Rect)=默认值|矩形纹理|
|名称("显示名称",Cube)=默认值|立方体纹理，CubeMap|

#### SubShader的标签

**Tags**主要设置渲染队列，渲染类型，阴影和投影 [ShaderLab:SubShader Tags](https://docs.unity3d.com/Manual/SL-SubShaderTags.html)

```
SubShader{
	Tags{"TagName1" = "Value1" "TagName2" = "Value2" }
}
```

**TagName1**和**TagName2**为标签名，常用为**Queue**和**RenderType**, **Value1**和**Value2**为变量， 具体可以参照官网文档。

#### Press的标签

主要是设置**LightMode** [ShaderLab: Pass Tag](https://docs.unity3d.com/Manual/SL-PassTags.html)

还有一些其他的参数，比如剔除&深度测试，透明测试，深度，混合等。 [ShaderLab: Pass](https://docs.unity3d.com/Manual/SL-Pass.html)



