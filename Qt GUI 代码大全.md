# 窗口设置

## QMainWindow类

- QMainWindow(QWidget \*parent = nullptr, Qt::WindowFlags flags = Qt::WindowFlags())

- void setCentralWidget(QWidget \*widget); //set the given widget to the main window's central widget

- void setFixedSize(int w, int h); //set the size of the widget

- void setWindowIcon(QIcon(QString filepath));

# 按钮和菜单

## QMenuBar类

- QMenuBar \*QMainWindow::menuBar() const
返回MainWindow的menu bar
//creates and returns an empty menu bar if the menu bar does not exist.

- QMenuBar::addMenu(QMenu \*menu)
- QMenuBar::addMenu(const QString& title)

## QMenu类
- addAction(QAction \*action)
- addSeparator()

## QAction类
可以看成是一个动作,连接到槽

- QAction(const QString &text, QObject \*parent = nullptr)

- 设置快捷键
```c
void QAction::setShortcuts(const QList<QKeySequence> &shortcuts)
```

- `->setStatusTip(tr("Start a new game"));` 设置说明
- ->setEnabled(false) 设置按钮激活状态
- connect(aboutAction, &QAction::triggered, this, &MainWindow::about);



# 文件交互

## QFileDialog类
用于打开文件选择窗口

```c
QString fileName = QFileDialog::getOpenFileName(this, tr("Open File"),                                       "/home",  //文件夹目录
tr("Images (*.png *.xpm *.jpg)"));
```

## QFileInfo类
用于获取文件的相关信息,比如后缀名等等
```c
QString extension = fileInfo.suffix().toLower();  // 获取小写的文件后缀名
```

## QFile类

- QFile(QString filename, QObject \*parent*);
- .setFileName(filePath);

## QTextStream
文本流,用于读取数据
QTextStream::QTextStream(FILE \*fileHandle, QIODevice::OpenMode openMode = QIODevice::ReadWrite)
```c
QTextStream in(&file);
```

- 逐行读取
```c
while(!in.atEnd()){
    QString line = in.readLine(); // 读取一行
     ... // 逐行处理
    }
    file.close();
```

# QMessageBox类

```c
if (QMessageBox::Yes == QMessageBox::information(NULL,
    tr("Game Over"), tr("Again?"),
    QMessageBox::Yes | QMessageBox::No,
    QMessageBox::Yes)) {
    
    game->newGame();
    }
    else{
        exit(0);
    }
}
```

# 绘图

## QPixmap类
贴图,纹理

- QPixmap::QPixmap(int width, int height)
//注释:上面尚未fill with color

- QPixmap::QPixmap(const QString &fileName, const char \*format = nullptr, Qt::ImageConversionFlags flags = Qt::AutoColor)

- void QPixmap::fill(const QColor &color = Qt::white)

## QPainter类

- QPainter(QPaintDevice \*device)
例如
```c
//QPixmap bg(TILE_SIZE, TILE_SIZE);
QPainter p(&bg);
QPainter p(this);
```

- setBrush(const QBrush)
- setPen(QPen)
- drawRect(int x, int y, int width, int height);
- drawLine
- drawPath(const QPainterPath &path) //current pen
- void QPainter::drawPolygon(const QPolygonF &points, Qt::FillRule fillRule = Qt::OddEvenFill)

void QPainter::drawEllipse(const QRectF &rectangle)

- save 保存当前painter状态
- restore 恢复

- setRenderHint(QPainter::RenderHint hint, bool on = true)
设置绘画风格
比如
```c
painter->setRenderHint(QPainter::Antialiasing);
//带有边缘
```

void QPainter::fillRect(const QRectF]&rectangle, const QBrush&brush)
Fills the given rectangle with the brush specified.
```text
Alternatively, you can specify a QColor instead of a QBrush; the QBrush constructor (taking a QColor argument) will automatically create a solid pattern brush.
```
相应有 fillPath等等

## QBrush类

刷子,可以是纹理/颜色
style设置绘制的方式

## QPen类

```c
  pen.setStyle(Qt::DashDotLine);
  pen.setWidth(3);
  pen.setBrush(Qt::green);
  pen.setCapStyle(Qt::RoundCap);
  pen.setJoinStyle(Qt::RoundJoin);
  painter.setPen(pen)
```

## QPainterPath类

- addRect等等

- clear
- boundingRect
- capacity vs length

- connectPath(&path)

- contains(QPoint/QRect/QPainterPath)
- 对应的intersect(相交),而contain是包含(区域)

我的发现： contains边界问题，Point可而其它二不可

- lineTo
- cubicTo 画线

- moveTo
Moves the current position to (x, y) and starts a new subpath, implicitly closing the previous path.

// 将QPoint添加到QPainterPath中 path.moveTo(point1); path.lineTo(point2); path.lineTo(point3); path.lineTo(point4); // 闭合路径 
- path.closeSubpath(); 

- path.setFillRule(Qt::WindingFill);
- 1
```c
foreach(const QPoint& p, body){
    path.addRect(p.x(), p.y(), FILE_SIZE, FILE_SIZE);
}
```


# 游戏场景

## QGraphicsItem类

- setPos(x, y);

- setData(int key, const QVariant &value);
使用：serData(GD_Type, GO_Food);

- QRectF QGraphicsItem::boundingRect() const;
自定义，原虚函数
返回Item的边界
例子：return QRectF(-TILE_SIZE,  -TILE_SIZE, TILE_SIZE * 2, TILE_SIZE * 2 );

- （pure virtual） void QGraphicsItem::paint(QPainter \*painter, const QStyleOptionGraphicsItem \*option, QWidget \*widget)
原虚函数，被QGraphicsView调用
例子：
```c
painter->save();
painter->setRenderHint(QPainter::Antialiasing);
painter->fillPath(shape(), Qt::red);
painter->restore();
```

- (virtual) QPainterPath shape()const
例子:
```c
QPainterPath p;
p.addEllipse(QPointF(TILE_SIZE / 2, TILE_SIZE / 2), FOOD_RADIUS, FOOD_RADIUS);
    return p;
```

- QPointF mapFromScene(const [QPointF](../qtcore/qpointf.html) &point) const
将Scene坐标系中的坐标映射到本Item坐标系中的点坐标

- void QGraphicsItem::advance(int phase)
phase = 0 预更新
phase = 1 更新
用于更新Item相关逻辑
```c
void Snake::advance(int step)
{
    if (!step) {
        return;
    }
    if (tickCounter++ % speed != 0) {
        return;
    }
    if (moveDirection == NoMove) {
        return;
    }
    if (growing > 0) {
		QPointF tailPoint = head;
        tail << tailPoint;
        growing -= 1;
    } else {
        tail.removeFirst();
        tail << head;
    }
    switch (moveDirection) {
        case MoveLeft:
            moveLeft();
            break;
        case MoveRight:
            moveRight();
            break;
        case MoveUp:
            moveUp();
            break;
        case MoveDown:
            moveDown();
            break;
    }
    setPos(head);
    handleCollisions();
}
```

- void QGraphicsItem::setPos(const QPointF &pos)
Sets the position of the item to pos, which is in parent coordinates.

- 碰撞检测
```c
QList<QGraphicsItem*> QGraphicsItem::collidingItems(Qt::ItemSelectionMode mode = Qt::IntersectsItemShape) const
```
可用之前设置的data判断与哪个物体的碰撞

## QGraphicsScene类 
//用于图形存放

- QGraphicsScene(QObject* parent = nullptr);
构造函数可用：
...scene(new QGraphicsScene(this));

- setSceneRect(x, y, w, h);//设置scene的位置
//使用实例：scene->setSceneRect(-100, -100, 200, 200);

- void addItem(QGraphicsItem \*item);
void removeItem(QGraphicsItem \*item);

- void QObject::installEventFilter(QObject \*filterObj);
//设置事件过滤器, filterObj会拦截并处理this的实践
例子: scene.installEventFilter(this);

- (virtual)
bool QObject::eventFilter(QObject \*object, QEvent \*event)
实现拦截处理函数
例子
```c
if (event->type() == QEvent::KeyPress) {
    handleKeyPressed((QKeyEvent *)event); //自定义的按键处理函数
    return true;//返回已处理
} else {
    return QObject::eventFilter(object, event);
    //不处理
    }
```

 - connect(&timer, SIGNAL(timeout()), &scene, SLOT(advance()));
 用于定时刷新界面

## QGraphicsView类

- QGraphicsView(scene, this);

- void fitInView(QRect, Qt::AspectRatioMode aspectRatioMode = Qt::IgnoreAspectRatio);
缩放视图矩阵并滚动滚动条，以确保场景矩形（`rect`）适应视口内

- setBackgroundBrush(QBrush(QPixmap)); //设置背景

