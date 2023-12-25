
# 前言

本文并不是严格的教程,只是笔者考试一败涂地后的碎碎念

# 一. 项目背景

从文件读入数据, 文件共有M + 1行, 数据格式如下:
```text
m n
a(0,0) a(0,1) ... a(0,n-1)
...
a(m-1,0) a(m-1,1) ... a(m-1,n-1)
```
m为地图的行数, n为地图的列数, 
a(x,y)表示第x行第y列的数值:
- =0, 用绿色表示
- =1, 用红色表示

# 二. 代码实现

## 1. 指针数组创建

```c++
class MainWindow : public QWidget
{
	...
	int **map;
	int m, n;
};
void MainWindow::load()
{
	map = new int*[m]; // m行
	for(int i = 0; i < m; ++i){
		//注意, 这里是对刚刚创建好的每一行进行操作, 所以循环到m
		map[i] = new int[n];
	}
}
```

## 2. gui坐标和数组的对应问题

然后就是本文的核心了, 也是最容易弄错的地方. 由于存地图的时候先存行数后存列数, 所以每个坐标(x, y)表示的是x行y列, 然而在绘图的时候用的是xy坐标, 也就是说, 横坐标对应的是列数, 纵坐标对应的是行数, 和存储正好相反. 因此paint函数应该如下:
```c++
void MainWindow::paintEvent(QPaintEvent *event)
{
	QPainter painter(this);
	const int FILE_SIZE = 60;
	for(int y = 0; y < m; ++y){//先行,先y
		for(int x = 0; x < n; ++x){//后列,后x
			switch(map[x][y]){
			case 0:
			painter.fillRect(x * FILE_SIZE, y * FILE_SIZE, FILE_SIZE, FILE_SIZE, Qt::green);
				break;
			case 1:
			painter.fillRect(x * FILE_SIZE, y * FILE_SIZE, FILE_SIZE, FILE_SIZE, Qt::red);
				break;
			}	
		}
	}
}
```

否则绘制出来的图形会是轴对称的,长宽颠倒.