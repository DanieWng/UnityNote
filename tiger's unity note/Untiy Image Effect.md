#Untiy Image Effect

</br>
</br>

## BaseEffectMesh
*	基础概念
	1. 重写函数

		```c sharp
		public override void ModifyMesh(VertexHelper vh)
		```
	
	2. VertexHelper参数类型
		
		官方文档说明：
		
		`A utility class that can aid in the generation of meshes for the UI.`
		
		即unity提供的简化网格操作的辅助类，它的接口也很简单，诸如添加顶点，添加三角形，添加Quad等.