# 颜色渐变

看QGradient

- QLinearGradient线性渐变![img](qgradient-linear.png)

- QRadialGradient半径渐变![img](qgradient-radial.png)

- QConicalGradient角度渐变![img](qgradient-conical.png)

  

  条带渐变：

|       |        |                   |
| ----- | ------ | ----------------- |
| start | 0      | rgba(1,0,0,1)     |
| stop  | 0.3332 | rgba(1,0,0,1)     |
| stop  | 0.3334 | **rgba(0,1,0,1)** |
| stop  | 0.6666 | **rgba(0,1,0,1)** |
| stop  | 0.6668 | *rgba(0,0,1,1)*   |
| stop  | 1      | *rgba(0,0,1,1)*   |

对，两个一组，规定起止，另一组的起始点是上一组的终点

算法渐变：stop:0 rgba(1,0,0,1)​ stop:1 (0,1,0,1)

## 更多效果见QtDesigner的样式表