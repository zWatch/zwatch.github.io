---
title: 场景扫描特效
date: 2019-10-11 13:30:55
updated:
tags:
    - ue4 
    - 特效
    - 蓝图
    - C++
categories:
    - ue4
    - 特效
---
n1：用到了Stencil，设置物体的Stencil值将改变被扫描物体的颜色  

1. 创建一个PostProcessVPostProcessVolume（后加工）
2. 创建材质蓝图M_ScanEffact，
    将Material Domain 更改为Post Process
    将Post Process Material 的 Blendable Location(可混合位置)更改为Before Tonemapping(调色前)
3. 在蓝图里创建SphereMask（球面掩模）节点。 -》 给那个唯一亮着的
    获取Absolute World Position -》 A
    获取Camera Position -》 B
    添加Param（参数）命名为Radius -》 Radius
    添加Param 命名为Hardness -》 Hardness  
4. 创建M_ScanEffact的材质球，将其添加到相机

5. 将SphereMask节点的输出改道路过Sine
    //这时Hardness决定环的宽，Radius决定
6. 将Sine出来的转到Clamp （min max）

7. 添加个Texture Sample（纹理采样）, 输出与Clamp的输出相乘，传给
    //这是预览会非常奇怪
8. 添加WorldAlignedTexture,输入7的Texture Sample{右键转换至Texture Object}
    添加Param 【Size】-》 Texture Size
    -》 WorldPoistion
9. new ComponentMask 【Mask（R G）】
    8中的XYZ Texture 输入

10. 9中的输出与7中Clamp 输出相乘
    Size设置为100，//此时Radius 为921， Hardness为0.49

11. 添加Scene Texture
        将Umaterial Expression Scene Texture 中的Scene Texture Id 更改为WorldNormal
        Color 输出至8中的WorldAlignedTexture 的 World Space Normal

12. 添加Lerp节点，将Mask与Clamp的乘积输出至此节点的Alpha

13. 复制SceneTexture WorldNormal ， 将Scene Texture Id 设置为PostProcessInput
14. 添加Compont Mask
    13中的Color输出至此
    输出至12的A
15. 12的输出至E*** Color

16. 添加Param[Scan Color]输出至12的Lerp的B
    将Scan Color 设置为【22，2，0，0】

//告一段落


17. 项目设置里搜索custom ste找到Postprocessing 的Custom Depth-Stencil Pass更改至Enable with Stencil

18. 添加SceneTexture{同上设置为CustomStencil}
    Color给E*** Color
    //可以找几个物体将其Reddering 的Render CustomDepth设置为true
    //可以找几个物体将其Reddering 的CustomDepth Stencil设置为1

19. 创建BitMask，
    0-》Bit（s）
    
20. BitMask的函数如下
    ```cpp
    if.A=Input_BitMask(Scalar)
    Divide.A=Input_Bit(Scalar)
    Divide.B=255
    if.B=Divide
    //return A==B;
    if(A==B){
        return 1;
    }else{
        return 0;
    }
    ```
        
21. 添加一个Mask（R）， 一个Divide（，255）
    18中的Color-》Mask（R） -》  Divide.A 
    Divide -> 19.BitMask(s)
    19.Result -> E*** Color

22 添加一个Lerp
    12.Lerp->this.Lerp.A
    19.Result -> this.Lerp.Alpha
    this.Lerp -> E*** Color

23. 创建Param（Vec4）[H1]
    this.H1->22.B
    将H1设置为（2，0，0，0）进行预览

24. 添加Lerp， 将22.Lerp-》this.Lerp.B
    this.Lerp->E*** Color
    14->this.Lerp.A

至此达到视频的18：23





