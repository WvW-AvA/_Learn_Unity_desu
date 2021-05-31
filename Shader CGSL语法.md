尝试写了写shader，发现还蛮有意思的。  
就是有些语义比较陌生，故记录下来，以供以后查询。

```

Shader "Practice/01"//指定shader路径和名字
{
	Properties
	{
		_Color("Color",Color) = (1,1,1,1)
		//定义参数 param name("name in inspector",param type)=initialize;
			_Vector("Vector",vector) = (1,2,3,4)
			_Range("Range",Range(1,11))=6
			_2D("2DTexture",2D)="white"{}
			_cube("Cube",Cube)="white"{}
			_3D("3D",3D) = "white"{}
	}
	SubShader//子Shader,若无法运行此SubShader则执行下个SubShader
	{
		pass //在pass块中编写代码，类比函数
		{

		CGPROGRAM//使用CG语言编写shader
			float4 _Color;
			//float,half,fixed也是小数定义，范围不同
			//float 32 bit
			//half  16 bit range -60k to 60k
			//fixed 11 bit range -2 to 2. which is used in color; 

			float4 _Vector;
			sampler2D _2D;

		ENDCG

		
		}
	}
	Fallback"Mobile/VertexLit"//备用shader方案，当所有SubShader都不可用时执行
}



Shader "Practice/02"
{
	Properties
	{

	}
	SubShader
	{
		pass
		{
		CGPROGRAM
#pragma vertex vert
			//定义顶点函数的函数名
			//基本作用：完成顶点坐标从模型空间到剪裁空间的转换
#pragma fragment frag
			//定义片面函数的函数名
			//基本作用：返回模型对应的屏幕上的每一个像素的颜色值
			//这两个函数很特殊，返回值和参数可以是不固定的
		float4 vert(float4 v : POSITION) :SV_POSITION//：通过语义告诉系统这个参数的意义。SV_POSITION指剪裁空间下的坐标，POSITION指顶点坐标
		{

			return UnityObjectToClipPos(v);//mul()矩阵相乘,v与UNITY_MATRIX_MVP相乘能将v转换为剪裁空间的坐标。
		}
		float4 frag() : SV_Target
		{
			return fixed4(0,0,1,1);
		}
		ENDCG
		}
	}
	Fallback"Mobile/VertexLit"
}
```
语义集：


从application 传递到vertex 函数可使用的语义  
POSITION    顶点坐标（模型空间下）float4  
NORMAL		法线向量 float3  
TANGENT       切线    float3  
TEXCOORD 0~n      纹理坐标  float4  
COLOR		颜色数据 float4

从vertex 函数传递到fragment函数可使用的语义  
SV_POSITION  剪裁空间中的顶点坐标，一般由系统调用
