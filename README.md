# FamilyTreeView
### 家谱树绘制Demo，主要使用自定义ViewGroup，和使用canvas进行划线，现阶段实现了自己、配偶、兄弟姐妹、父母、祖父母、外祖父母、子女和儿媳妇，女婿以及孙子，共五代的绘制，加入了touch事件可以移动，代码可能写得相对比较死，基本只能使用于家谱展示。还增加了有养父母的控件，其实是拷贝原来的改的（公司项目奇葩需求，这个控件不做维护更新），没有养父母才是正常的关系布局图（这个做更新维护）。

## 运行流程

#### 1、首先通过assets获取Json
![assets](https://raw.githubusercontent.com/ssj64260/FamilyTreeView/master/image/QQ%E6%88%AA%E5%9B%BE20170602231531.png)

部分Json
![Json](https://raw.githubusercontent.com/ssj64260/FamilyTreeView/master/image/QQ%E6%88%AA%E5%9B%BE20170602231651.png)

#### 2、将Json转成对象列表
![对象](https://raw.githubusercontent.com/ssj64260/FamilyTreeView/master/image/QQ%E6%88%AA%E5%9B%BE20170602232244.png)

#### 3、将对象列表存进数据库
#### 4、查询数据库拼装带关系的对象
#### 5、将关系对象交给控件让控件绘制关系图

## 预览图
UI相对比较丑，用着先，以后再改

![静态预览图](https://raw.githubusercontent.com/ssj64260/FamilyTreeView/master/image/Screenshot_2017-06-03-00-19-00-163_com.cxb.familytree.png)

![静态预览图](https://raw.githubusercontent.com/ssj64260/FamilyTreeView/master/image/Screenshot_2017-06-03-00-19-08-280_com.cxb.familytree.png)

![静态预览图](https://raw.githubusercontent.com/ssj64260/FamilyTreeView/master/image/Screenshot_2017-06-02-23-08-34-249_com.cxb.familytree.png)


## 思路说明
#### 1、代码执行流程
	1.1、首先当然时初始化数据，包括家庭成员对象、画笔等等。
	1.2、然后就是根据家庭成员对象，测量布局后ViewGroup的宽高，最大高度是固定的最大一共五代人，宽度分别计算每一代的最大宽度来得出。
	1.3、初始化View，把家庭成员的View都初始化。然后调用invalidate()更新界面。
	1.4、在onMeasure测量家庭成员View的宽高。
	1.5、在onLayout里编排好每个成员的具体位置。
	1.6、在onDraw方法里，利用canvas画出家庭成员关系的连线。
	1.7、最后在onTouchEvent方法写下触摸事件移动ViewGroup，在onInterceptTouchEvent写下移动时是否拦截成员View的点击事件。
  
#### 2、onLayout方法的定位处理
	2.1、首先订好了自己的View的位置,自己的位置位于整个布局的中心。
	2.2、如果有配偶则将配偶排在自己的右边。
	2.3、如果有兄弟姐妹那么将兄弟姐妹按序一字排在自己的左边。
	2.4、如果只有父亲或父母，则排在自己正上方，如果双亲都有，那么父亲排在自己左上方，母亲排在自己右上方。
	2.5、祖父母和外祖父母的原理和父母的一样。不过祖父母是相对父亲来排，而外祖父母是相对母亲来排。
	2.6、这里要先排孙子孙女，因为我的孙子孙女的位置决定了我的子女的位置。
	2.7、排好孙子孙女，就按照孙子孙女的位置确定子女的位置。
	
#### 3、onDraw方法的画线处理
	3.1、首先是自己和配偶的连线，都是canvas，Paint，Path的功夫，不懂就百度。
	3.2、然后是父母以及祖父母的连线，这里并不难我就不多说了，代码写得很清楚了。
	3.3、接着就是兄弟姐妹的连线，直接从最左边兄弟的X坐标，练到自己View的X坐标，这里是偷懒的连线方式。(￣▽￣)")
	3.4、最后就是子女和孙子女的画线，这里也是先连孙子女，把同父母的孙子孙女先连线，然后再连孙子女和子女的线，接着把子女连起来，最后就是我和子女的连线。

#### 4、Github：[https://github.com/ssj64260/FamilyTreeView](https://github.com/ssj64260/FamilyTreeView)
