1. 不要在单例模式的构造函数中与非单例所在类的信号函数或者槽函数进行连接<p> Do Not connect a signal or slot that not in the Signleton Class int the Singleton Class.<p>

2. 这里是一个 QSlider 的样式表<p>Here is a Style Sheet of a QSlider.<p>

        "QSlider::groove:horizontal {border: 2px solid #999999;border-radius:4px; height: 15px;margin: 0px 0;left: 12px; right: 12px;}"
        "QSlider::handle:horizontal {border: 2px solid #5c5c5c;border-radius:8px; background:yellow;width: 40px;margin: -10px -2px -10px -2px;}"
        "QSlider::sub-page:horizontal{background: qlineargradient(spread:pad, x1:0, y1:1, x2:0, y2:0, stop:0 rgba(27, 5, 27, 255), stop:0.25 rgba(99, 20, 102, 255), stop:0.5 rgba(154, 30, 158, 255), stop:1 rgba(173, 57, 176, 255));}"
        "QSlider::add-page:horizontal{background:red}"
	 ![QSlider](image/QSliderStyleSheet.png)<p>
	<p>注意：代码 **margin: -10px -2px -10px -2px;** 可以实现滑条的滑块比滑条大。
	<p>Note: The code **margin: -10px -2px -10px -2px;** could implement a slider handle that is larger than the slider groove

3. 这里是一个 QStyledItemDelegate,它可以帮助你实现一个同时的存在右对齐图标和你设置的样式表的 QListWidget/QListView <p>Here is a QStyledItemDelegate,it could help you finish a QListWidget/QListView with a right-aligned icon and the style sheet you set that exists at the same time.
	
	class myDelegate : public QStyledItemDelegate
	{
		public:
		void paint(QPainter *painter, const QStyleOptionViewItem &option, const QModelIndex &index) const{
			QStyleOptionViewItemV4 opt = option;
		    initStyleOption(&opt,index);
		    opt.decorationPosition = QStyleOptionViewItem::Right;
		    opt.font.setBold(true);
    
		    const QWidget *widget = opt.widget;
		    QStyle *style = widget ? widget->style() : QApplication::style();
		    style->drawControl(QStyle::CE_ItemViewItem,&opt,painter,widget);
		  }
	};
	
4. 如果你需要制作 makeqpf 请参考这篇文章 [http://blog.csdn.net/u014213012/article/details/53352645](http://blog.csdn.net/u014213012/article/details/53352645) ,并在 Ubuntu 16.04 下按步骤执行 <p>If you want to make a "makeqpf", the website  [http://blog.csdn.net/u014213012/article/details/53352645](http://blog.csdn.net/u014213012/article/details/53352645) could help you ,please follow the web and make it in Ununtu 16.04 .<p>

5. 如果在单例模式类的构造函数中，将另一个实例化对象 通过 moveTOThread() 函数移动到另一个线程中，如果在构造函数中 start 该线程，则该 程序无法开机自动运行。<p> If the other instanced object is moved to another thread through the moveTOThread() function in the constructor of the Singleton Class,the program can not start while you set the program "run while boot".

6. QPixmap QPixmap::grabWidget ( QWidget * widget, const QRect & rectangle ) [static] 可以对页面进行实时截图<p>Function QPixmap QPixmap::grabWidget ( QWidget * widget, const QRect & rectangle ) [static] could make a screenshot of the widget.

7. 如果一个 QLabel 的 是不规则形状的（比如圆角矩形），如果想让 QLabel 中插入的图片呈现出完全一样的形状，可以使用下面的代码。addRoundedRect() 函数可以换成其他函数，以实现不同的形状。<p>If a QLabel is an irregular shape (such as a rounded rectangle), you can use the following code if you want the picture inserted in QLabel to show exactly the same shape. The addRoundedRect () function can be replaced by other functions to implement different shapes.

	void paintEvent(QPaintEvent *e)
	{
		if(pixmap())
		{
			QPainter painter(this);
			painter.setRenderHints(QPainter::Antialiasing | QPainter::SmoothPixmapTransform);
			QPainterPath path;
			path.addRoundedRect(this->rect(), 10, 10, Qt::AbsoluteSize);
			// replace 10 with the size you want.
			painter.setClipPath(path);
			painter.drawPixmap(0, 0, width(), height(), *pixmap());
		}
		else
		{
			 ClickableQLable::paintEvent(e);
		}     
	}
	
8. 如果需要发送自定义事件到 QT 事件循环中，可以使用如下代码先注册 自定义事件。<p>If you want to send a customed event to the Event Loop of QT Application, you can use the code to regist QCustomEvent first.<p>

		QEvent::Type myEventType = QEvent::Type(QEvent::registerEventType(1200));
		//you can replace 1200 with the number you want. using #define is better;
		
    <p>然后使用如下代码 发送到事件循环中。<p> Then send it to the Event Lopp using the code as follow.<p>
	
		QApplication::postEvent(this->parent()->parent(), new QEvent(myEventType));	
	<p>使用如下代码进行接收处理<p> Using the code as folloe to process the event.<p>
		
		if(e->type() == 1200)//using the number you defined
		{
			qDebug() << "process the event";
		}
	
	
9. 继承 QListWidgetItem ,添加 setEnable(bool isEnable) 方法，可以实现 QListWidget/QListView 中的 item 是否允许被点击。<p>Inherited QListWidgetItem, add setEnabled (bool isEnabled) method, you can set QListWidget / QListView in the item is allowed to be click or not.

	void setEnbale(bool isEnable)
	{
	    if(false != isEnable)
	        this->setFlags(this->flags() | Qt::ItemIsEnabled);
	    else
	        this->setFlags(this->flags() & ~Qt::ItemIsEnabled);
	}
	
10. 如果 QtCreator 没有代码自动补全，请检查设置 工具-->选项-->文本编辑器-->补全。如果还是没有代码提示，将输入法切换到英文输入法。中文输入法的英文输入也不行。<p>
 While QtCreator cannot completing code,Please check at tools-->options-->TextEditor-->Completion. If this cannot fix it, please check your input method, I think it could use only when the input method is English.

