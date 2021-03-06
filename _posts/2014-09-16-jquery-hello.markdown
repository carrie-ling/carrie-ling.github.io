---
layout: postjquery
title:  "Node!"
date:   2014-09-16 17:46:13
tags: [postjquery]
categories: jekyll update
---

# Node节点对象 #


DOM的最小单位是节点（node），一个文档的树形结构就是由各种不同的节点组成。

对于HTML文档，节点有以下类型：

节点|名称|含义
----|----|----
DOCUMENT_NODE | 文档节点 | 整个文档（window.document）
ELEMENT_NODE | 元素节点 | HTML元素（比如&lt;body&gt;、&lt;a&gt;等）
ATTRIBUTE_NODE | 属性节点| HTML元素的属性（比如class="right"）
TEXT_NODE | 文本节点 | HTML文档中出现的文本
DOCUMENT_FRAGMENT_NODE | 文档碎片节点 | 文档的片段
DOCUMENT_TYPE_NODE | 文档类型节点 | 文档的类型（比如&lt;!DOCTYPE html&gt;）

浏览器原生提供一个Node对象，上面所有类型的节点都是Node对象派生出来的，也就是说它们都继承了Node的属性和方法。

### Node对象的属性

**（1）nodeName属性和nodeType属性**

nodeName属性返回节点的名称，nodeType属性返回节点的常数值。具体的返回值，可查阅下方的表格。

类型 | nodeName | nodeType 
-----|----------|---------
DOCUMENT_NODE | #document | 9
ELEMENT_NODE | 大写的HTML元素名 | 1
ATTRIBUTE_NODE | 等同于Attr.name | 2
TEXT_NODE | #text | 3
DOCUMENT_FRAGMENT_NODE | #document-fragment | 11
DOCUMENT_TYPE_NODE | 等同于DocumentType.name |10

以document节点为例，它的nodeName属性等于#document，nodeType属性等于9。

	javascript
	
	document.nodeName // "#document"
	document.nodeType // 9


通常来说，使用nodeType属性确定一个节点的类型，比较方便。

```javascript

document.querySelector('a').nodeType === 1
// true

document.querySelector('a').nodeType === Node.ELEMENT_NODE
// true

```

上面两种写法是等价的。

**（2）nodeValue属性**

Text节点的nodeValue属性返回文本内容，而其他五类节点都返回null。所以，该属性的作用主要是提取文本节点的内容。

**（3）childNodes属性和children属性**

childNodes属性返回一个NodeList对象，该对象的成员是父节点的所有子节点，注意返回的不仅包括元素节点，还包括文本节点以及其他各种类型的子节点。如果父对象不包括任何子对象，则返回一个空对象。

{% highlight javascript %}

var ulElementChildNodes = document.querySelector('ul').childNodes;

{% endhighlight %}

children属性返回一个类似数组的对象，该对象的成员为HTML元素类型的子节点。如果没有HTML元素类型的子节点，则返回一个空数组。

childNodes属性和children属性返回的节点都是动态的。一旦原节点发生变化，立刻会反映在返回结果之中。

**（4）指向其他节点的属性**

以下属性指向其他的相关节点，用于遍历文档节点时非常方便。

- firstChild：第一个子节点。
- lastChild：最后一个子节点。
- parentNode：父节点。
- nextSibling：下一个同级节点。
- previousSibling：上一个同级节点。
- firstElementChild：第一个类型为HTML元素的子节点。
- lastElementChild：最后一个类型为HTML元素的子节点。
- nextElementSibling：下一个类型为HTML元素的同级节点。
- previousElementSibling：上一个类型为HTML元素的同级节点。

### Node对象的方法

Node对象有以下方法：

- appendChild()
- cloneNode()
- compareDocumentPosition()
- contains()
- hasChildNodes()
- insertBefore()
- isEqualNode()
- removeChild()
- replaceChild()

appendChild()方法用于在父节点的最后一个子节点后，再插入一个子节点。

{% highlight javascript %}

var elementNode = document.createElement('strong');
var textNode = document.createTextNode(' Dude');

document.querySelector('p').appendChild(elementNode);
document.querySelector('strong').appendChild(textNode);

{% endhighlight %}

insertBefore()用于将子节点插入父节点的指定位置。它接受两个参数，第一个参数是所要插入的子节点，第二个参数是父节点下方的另一个子节点，新插入的子节点将插在这个节点的前面。

{% highlight javascript %}

var text1 = document.createTextNode('1');
var li = document.createElement('li');
li.appendChild(text1);

var ul = document.querySelector('ul');
ul.insertBefore(li,ul.firstChild);

{% endhighlight %}

removeChild() 方法用于从父节点移除一个子节点。

{% highlight javascript %}

var divA = document.getElementById('A');
divA.parentNode.removeChild(divA);

{% endhighlight %}

replaceChild()方法用于将一个新的节点，替换父节点的某一个子节点。它接受两个参数，第一个参数是用来替换的新节点，第二个参数将要被替换走的子节点。

{% highlight javascript %}

var divA = document.getElementById('A');
var newSpan = document.createElement('span');
newSpan.textContent = 'Hello World!';
divA.parentNode.replaceChild(newSpan,divA);

{% endhighlight %}

cloneNode()方法用于克隆一个节点。它接受一个布尔值作为参数，表示是否同时克隆子节点，默认是false，即不克隆子节点。

{% highlight javascript %}

var cloneUL = document.querySelector('ul').cloneNode(true);

{% endhighlight %}

需要注意的是，克隆一个节点，会丧失定义在这个节点上的事件回调函数，但是会拷贝该节点的所有属性。因此，有可能克隆一个节点之后，DOM中出现两个有相同ID属性的HTML元素。

contains方法检查一个节点是否为另一个节点的子节点。

{% highlight javascript %}

document.querySelector('html').contains(document.querySelector('body'))
// true

{% endhighlight %}

isEqualNode()方法用来检查两个节点是否相等。所谓相等的节点，指的是两个节点的类型相同、属性相同、子节点相同。

{% highlight javascript %}

var input = document.querySelectorAll('input');
input[0].isEqualNode(input[1])

{% endhighlight %}

### NodeList对象

当使用querySelectorAll()方法选择一组对象时，会返回一个NodeList对象（比如document.querySelectorAll('*')的返回结果）或者HTMLCollection对象（比如document.scripts）。它们是类似数组的对象，即可以使用length属性，但是不能使用pop或push之类数组特有的方法。

## Element对象

### 属性

每一个HTML标签元素，都会转化成一个Element对象节点。所有的Element节点的nodeType属性都是1，但是不同标签生成的节点是不一样的。JavaScript内部使用不同的构造函数，生成不同的Element节点，比如a标签的节点由HTMLAnchorElement()构造函数生成，button标签的节点由HTMLButtonElement()构造函数生成。因此，Element对象不是一种对象，而是一组对象。

Element对象特有的属性：

- innerHTML
- outerHTML
- textContent
- innerText
- outerText
- firstElementChild
- lastElementChild
- nextElementChild
- previousElementChild
- children
- tagName
- dataset
- attributes

**（1）innerHTML属性，outerHTML属性，textContent属性，innerText属性，outerText属性**

innerHTML属性用来读取或设置某个节点内的HTML代码。

outerHTML属性用来读取或设置HTML代码时，会把节点本身包括在内。

textContent属性用来读取或设置节点包含的文本内容。

innerText属性和outerText属性在读取元素节点的文本内容时，得到的值是不一样的。它们的不同之处在于设置一个节点的文本属性时，outerText属性会使得原来的元素节点被文本节点替换掉。

**（2）tagName属性**

tagName属性返回该节点的HTML标签名，与nodeName属性相同。

{% highlight javascript %}

document.querySelector('a').tagName // A

document.querySelector('a').nodeName // A

{% endhighlight %}

从上面代码可以看出，这两个属性返回的都是标签名的大写形式。

**（3）attributes属性**

该属性返回一个数组，数组成员就是Element元素包含的每一个属性节点对象。

{% highlight javascript %}

var atts = document.querySelector('a').attributes;

for(var i=0; i< atts.length; i++){
	console.log(atts[i].nodeName +'='+ atts[i].nodeValue);
}

{% endhighlight %}

**（4）textcontent属性**

该属性返回Element节点包含的所有文本内容。它通常用于剥离HTML标签，还用于返回&lt;script&gt;and&lt;style&gt;标签所包含的代码。

{% highlight javascript %}

document.body.textContent

{% endhighlight %}

如果对document或者doctype节点使用该属性，会返回null。

textcontent属性的作用与innerText属性很相近，但是有以下几点区别：

- innerText受CSS影响，textcontent没有这个问题。比如，如果CSS规则隐藏了某段文本，innerText就不会返回这段文本，textcontent则照样返回。

- innerText返回的文本，会过滤掉空格、换行和回车键，textcontent则不会。

- innerText属性不是DOM标准的一部分，Firefox浏览器甚至没有部署这个属性，而textcontent是DOM标准的一部分。

另外，该属性是可写的，可以用它设置Element节点的文本内容。但是这样一来，原有的子节点会被全部删除。

### className属性和classList属性

className属性和classList属性都返回HTML元素的class属性。不同之处是，className属性返回一个字符串，每个class之间用空格分割，classList属性则返回一个类似数组的对象，每个class就是这个对象的一个成员。

{% highlight html %}

<div class="one two three" id="myDiv"></div>

{% endhighlight %}

上面这个div节点对象的className属性和classList属性，分别如下：

{% highlight javascript %}

document.getElementById('myDiv').className
// "one two three"

document.getElementById('myDiv').classList
// {
//	0: "one"
//	1: "two"
//	2: "three"
//	length: 3
//	}

{% endhighlight %}

从上面代码可以看出，classList属性指向一个类似数组的对象，该对象的length属性（只读）返回该节点的calss数量。

classList对象有一系列方法。

- add()：增加一个class。
- remove()：移除一个class。
- contains()：检查该DOM元素是否包含某个class。
- toggle()：将某个class移入或移出该DOM元素。
- item()：返回列表中某个特定位置的class。
- toString()：将class的列表转为字符串。

{% highlight javascript %}

myDiv.classList.add('myCssClass');

myDiv.classList.remove('myCssClass');

myDiv.classList.toggle('myCssClass'); // myCssClass被加入

myDiv.classList.toggle('myCssClass'); // myCssClass被移除

myDiv.classList.contains('myCssClass'); // 返回 true 或者 false

myDiv.classList.item(0);

myDiv.classList.toString();

{% endhighlight %}

各大浏览器（包括IE 10）都支持classList属性。

### html元素

html元素是网页的根元素，document.documentElement就指向这个元素。

**（1）clientWidth属性，clientHeight属性**

这两个属性返回视口（viewport）的大小，单位为像素。所谓“视口”，是指用户当前能够看见的那部分网页的大小

document.documentElement.clientWidth和document.documentElement.clientHeight，基本上与window.innerWidth和window.innerHeight同义。只有一个区别，前者不将滚动条计算在内（很显然，滚动条和工具栏会减小视口大小），而后者包括了滚动条的高度和宽度。

**（2）offsetWidth属性，offsetHeight属性**

这两个属性返回html元素的宽度和高度，即网页的总宽度和总高度。

### dataset属性

dataset属性用于操作HTML标签元素的data-*属性。目前，Firefox、Chrome、Opera、Safari浏览器支持该API。

假设有如下的网页代码。

{% highlight html %}

<div id="myDiv" data-id="myId"></div>

{% endhighlight %}

以data-id属性为例，要读取这个值，可以用dataset.id。

{% highlight javascript %}

var id = document.getElementById("myDiv").dataset.id;

{% endhighlight %}

要设置data-id属性，可以直接对dataset.id赋值。这时，如果data-id属性不存在，将会被创造出来。

{% highlight javascript %}

document.getElementById("myDiv").dataset.id = "hello";

{% endhighlight %}

删除一个data-*属性，可以直接使用delete命令。

{% highlight javascript %}

delete document.getElementById("myDiv").dataset.id

{% endhighlight %}

IE 9不支持dataset属性，可以用 getAttribute('data-foo')、removeAttribute('data-foo')、setAttribute('data-foo')、hasAttribute('data-foo') 代替。

需要注意的是，dataset属性使用骆驼拼写法表示属性名，这意味着data-hello-world会用dataset.helloWorld表示。而如果此时存在一个data-helloWorld属性，该属性将无法读取，也就是说，data属性本身只能使用连词号，不能使用骆驼拼写法。

### 页面位置相关属性

**（1）offsetParent属性、offsetTop属性和offsetLeft属性**

这三个属性提供Element对象在页面上的位置。

- offsetParent：当前HTML元素的最靠近的、并且CSS的position属性不等于static的父元素。
- offsetTop：当前HTML元素左上角相对于offsetParent的垂直位移。
- offsetLeft：当前HTML元素左上角相对于offsetParent的水平位移。

如果Element对象的父对象都没有将position属性设置为非static的值（比如absolute或relative），则offsetParent属性指向body元素。另外，计算offsetTop和offsetLeft的时候，是从边框的左上角开始计算，即Element对象的border宽度不计入offsetTop和offsetLeft。

**（2） clientWidth属性和clientHeight属性**

这两个属性返回HTML元素的宽度和高度，在数值上等于内容本身+padding，不包括边框（border）。

{% highlight javascript %}

document.querySelector('div').clientWidth
document.querySelector('div').clientHeight

{% endhighlight %}

如果一个元素是可以滚动的，则clientWidth和clientHeight只计算它的可见部分的宽度和高度。

**（3）scrollHeight属性和scrollWidth属性**

这两个只读属性提供可滚动的HTML元素的总高度和总宽度。

{% highlight javascript %}

// <html>元素的总高度
document.documentElement.scrollHeight

// <body>元素的总高度
document.body.scrollHeight

{% endhighlight %}

**（4）scrollTop属性和scrollLeft属性**

这两个属性提供可滚动元素的可以滚动的高度和宽度。这两个属性是读写的。

{% highlight javascript %}

document.querySelector('div').scrollTop = 750;

{% endhighlight %}

上面代码将div元素的向下滚动750像素。

可滚动对象的高度和宽度，满足下面的公式。

{% highlight javascript %}

element.scrollHeight - element.scrollTop === element.clientHeight

{% endhighlight %}

### style属性

style属性用来读写页面元素的行内CSS属性，详见本章《CSS操作》一节。

### Element对象的方法

**（1）选择子元素的方法**

Element对象也部署了document对象的4个选择子元素的方法，而且用法完全一样。

- querySelector方法
- querySelectorAll方法
- getElementsByTagName方法
- getElementsByClassName方法

上面四个方法只用于选择Element对象的子节点。因此，可以采用链式写法来选择子节点。

{% highlight javascript %}

document.getElementById('header').getElementsByClassName('a')

{% endhighlight %}

各大浏览器对这四个方法都支持良好，IE的情况如下：IE 6开始支持getElementsByTagName，IE 8开始支持querySelector和querySelectorAll，IE 9开始支持getElementsByClassName。

**（2）elementFromPoint方法**

该方法用于选择在指定坐标的最上层的Element对象。

{% highlight javascript %}

document.elementFromPoint(50,50)

{% endhighlight %}

上面代码了选中在(50,50)这个坐标的最上层的那个HTML元素。

**（3）HTML元素的属性相关方法**

- hasAttribute()：返回一个布尔值，表示Element对象是否有该属性。
- getAttribute()
- setAttribute()
- removeAttribute()

**（4）matchesSelector方法**

该方法返回一个布尔值，表示Element对象是否符合某个CSS选择器。

{% highlight javascript %}

document.querySelector('li').matchesSelector('li:first-child')

{% endhighlight %}

这个方法需要加上浏览器前缀，需要写成mozMatchesSelector()、webkitMatchesSelector()、oMatchesSelector()、msMatchesSelector()。

**（5）scrollIntoView方法**

该方法用于将一个可滚动元素滚动到可见区域。

{% highlight javascript %}

document.querySelector('content').children[4].scrollIntoView();

{% endhighlight %}

scrollIntoView方法接受一个布尔值作为参数，默认值为true，表示滚动到HTML元素的上方边缘，如果该值为false，表示滚动到下方边缘。

### insertAdjacentHTML方法

insertAdjacentHTML方法可以将一段字符串，作为HTML或XML对象，插入DOM。

比如，原来的DOM结构如下：

{% highlight html %}

<div id="box1">
    <p>Some example text</p>
</div>
<div id="box2">
    <p>Some example text</p>
</div>

{% endhighlight %}

insertAdjacentHTML方法可以轻而易举地在上面两个div节点之间，再插入一个div节点。

{% highlight javascript %}

var box2 = document.getElementById("box2");

box2.insertAdjacentHTML('beforebegin', '<div><p>This gets inserted.</p></div>');

{% endhighlight %}

插入以后的DOM结构变成下面这样：

{% highlight html %}

<div id="box1">
    <p>Some example text</p>
</div>
<div><p>This gets inserted.</p></div>
<div id="box2">
    <p>Some example text</p>
</div>

{% endhighlight %}

insertAdjacentHTML方法接受两个参数，第一个是插入的位置，第二个是插入的节点字符串。关于插入的位置，可以取下面四个值。

- **beforebegin**：在指定元素之前插入，变成它的同级元素。
- **afterbegin**：在指定元素的开始标签之后插入，变成它的第一个子元素。
- **beforeend**：在指定元素的结束标签之前插入，变成它的最后一个子元素。
- **afterend**：在指定元素之后插入，变成它的同级元素。

需要注意的是，上面四个值都是字符串，而不是关键字，所以必须放在引号里面。

insertAdjacentHTML方法比innerHTML方法效率高，因为它不是彻底置换现有的DOM结构。所有浏览器都支持这个方法，包括IE 6。

### getBoundingClientRect方法

getBoundingClientRect方法返回一个记录了位置信息的对象，用于获取HTML元素相对于视口（viewport）左上角的位置以及本身的长度和宽度。

{% highlight javascript %}

var box = document.getElementById('box');

var x1 = box.getBoundingClientRect().left;
var y1 = box.getBoundingClientRect().top;
var x2 = box.getBoundingClientRect().right;
var y2 = box.getBoundingClientRect().bottom;
var w = box.getBoundingClientRect().width;
var h = box.getBoundingClientRect().height;

{% endhighlight %}

上面代码获取DOM元素之后，使用getBoundingClientRect方法的相应属性，先后得到左上角和右下角的四个坐标（相对于视口），以及元素的宽和高。所有这些值都是只读的。

注意，getBoundingClientRect方法的所有属性，都把边框（border属性）算作元素的一部分。也就是说，都是从边框外缘的各个点来计算。因此，width和height包括了元素本身+padding+border。

所有浏览器都支持这个方法，但是IE 6到8对这个对象的支持不完整。 

### table元素

表格有一些特殊的DOM操作方法。

- **insertRow()**：在指定位置插入一个新行（tr）。
- **deleteRow()**：在指定位置删除一行（tr）。
- **insertCell()**：在指定位置插入一个单元格（td）。
- **deleteCell()**：在指定位置删除一个单元格（td）。
- **createCaption()**：插入标题。
- **deleteCaption()**：删除标题。
- **createTHead()**：插入表头。
- **deleteTHead()**：删除表头。

下面是使用JavaScript生成表格的一个例子。

{% highlight javascript %}

var table = document.createElement('table');
var tbody = document.createElement('tbody');
table.appendChild(tbody);

for (var i = 0; i <= 9; i++) {
  var rowcount = i + 1;
  tbody.insertRow(i);
  tbody.rows[i].insertCell(0);
  tbody.rows[i].insertCell(1);
  tbody.rows[i].insertCell(2);
  tbody.rows[i].cells[0].appendChild(document.createTextNode('Row ' + rowcount + ', Cell 1'));
  tbody.rows[i].cells[1].appendChild(document.createTextNode('Row ' + rowcount + ', Cell 2'));
  tbody.rows[i].cells[2].appendChild(document.createTextNode('Row ' + rowcount + ', Cell 3'));
}

table.createCaption();
table.caption.appendChild(document.createTextNode('A DOM-Generated Table'));

document.body.appendChild(table);

{% endhighlight %}

这些代码相当易读，其中需要注意的就是insertRow和insertCell方法，接受一个表示位置的参数（从0开始的整数）。

table元素有以下属性：

- **caption**：标题。
- **tHead**：表头。
- **tFoot**：表尾。
- **rows**：行元素对象，该属性只读。
- **rows.cells**：每一行的单元格对象，该属性只读。
- **tBodies**：表体，该属性只读。

## Text节点

文档中的文本对应Text节点，通常使用Element对象的firstChild、nextSibling等属性获取文本节点，或者使用document对象的createTextNode方法创造一个文本节点。

{% highlight javascript %}

// 获取文本节点
var textNode = document.querySelector('p').firstChild;

// 创造文本节点
var textNode = document.createTextNode('Hi');
document.querySelector('div').appendChild(textNode);

{% endhighlight %}

注意，由于空格也是一个字符，所以哪怕只有一个空格，也会形成文本节点。

### 文本节点的属性

除了继承的属性，文本节点自身主要的属性是data，它等同于nodeValue属性，用来返回文本节点的内容。

{% highlight javascript %}

document.querySelector('p').firstChild.data

document.querySelector('p').firstChild.nodeValue

{% endhighlight %}

### 文本节点的方法

（1）文本编辑方法

- appendData()：在文本尾部追加字符串。
- deleteData()：删除子字符串，第一个参数为子字符串位置，第二个参数为子字符串长度。
- insertData()：在文本中插入字符串，第一个参数为插入位置，第二个参数为插入的子字符串。
- replaceData()：替换文本，第一个参数为替换开始位置，第二个参数为需要被替换掉的长度，第三个参数为新加入的字符串。
- subStringData()：获取子字符串，第一个参数为子字符串在文本中的开始位置，第二个参数为子字符串长度。

{% highlight javascript %}

var pElementText = document.querySelector('p').firstChild;

pElementText.appendData('!');
pElementText.deleteData(7,5);
pElementText.insertData(7,'Hello ');
pElementText.replaceData(7,5,'World');
pElementText.substringData(7,10));

{% endhighlight %}

（2）文本的分割与合并

- splitText()：将文本节点一分为二。
- normalize()：将毗邻的两个文本节点合并。

{% highlight javascript %}

// 将文本节点从第4个位置开始一分为二
document.querySelector('p').firstChild.splitText(4)

document.querySelector('p').firstChild.textContent
// 2

// 将毗邻的两个文本节点合并
document.querySelector('div').normalize()

document.querySelector('p').childNodes.length
// 1

{% endhighlight %}

## DocumentFragment节点

DocumentFragment节点代表一个完整的DOM树形结构，但是不属于当前文档，只存在于内存之中。操作DocumentFragment节点，要比直接操作文档快得多。它就像一个文档的片段，一般用于构建一个子结构，然后插入当前文档。

document对象的createDocumentFragment方法可以创建DocumentFragment节点，然后再可以使用其他DOM方法，添加子节点。

{% highlight javascript %}

var docFrag = document.createDocumentFragment();
var li = document.createElement("li");
li.textContent = "Hello World";
docFrag.appendChild(li);

document.queryselector('ul').appendChild(docFrag);

{% endhighlight %}

上面代码创建了一个DocumentFragment节点，然后将一个li节点添加在它里面，最后将DocumentFragment节点移动到原文档。

一旦DocumentFragment节点被添加进原文档，它自身就变成了空节点（textContent属性为空字符串）。如果想要保存DocumentFragment节点的内容，可以使用cloneNode方法。

{% highlight javascript %}

document.queryselector('ul').(docFrag.cloneNode(true));

{% endhighlight %}


- Louis Lazaris, [Thinking Inside The Box With Vanilla JavaScript](http://coding.smashingmagazine.com/2013/10/06/inside-the-box-with-vanilla-javascript/)
- David Walsh, [HTML5 classList API](http://davidwalsh.name/classlist)
- Derek Johnson, [The classList API](http://html5doctor.com/the-classlist-api/)
- Mozilla Developer Network, [element.dataset API](http://davidwalsh.name/element-dataset)
- David Walsh, [The element.dataset API](http://davidwalsh.name/element-dataset) 

jQuery是目前使用最广泛的JavaScript函数库。据[统计](http://w3techs.com/technologies/details/js-jquery/all/all)，全世界57.5%的网站使用jQuery，在使用JavaScript函数库的网站中，93.0%使用jQuery。它已经成了开发者必须学会的技能。

jQuery的最大优势有两个。首先，它基本是一个DOM操作工具，可以使操作DOM对象变得异常容易。其次，它统一了不同浏览器的API接口，使得代码在所有现代浏览器均能运行，开发者不用担心浏览器之间的差异。

## jQuery的加载

一般采用下面的写法，在网页中加载jQuery。

{% highlight html %}

<script type="text/javascript" src="//code.jquery.com/jquery-1.11.0.min.js"></script>
<script>
window.jQuery || document.write('<script src="js/jquery-1.11.0.min.js" type="text/javascript"><\/script>')
</script>

{% endhighlight %}

上面代码有两点需要注意。一是采用[CDN](http://jquery.com/download/#using-jquery-with-a-cdn)加载。如果CDN加载失败，则退回到本地加载。二是采用协议无关的加载网址，同时支持http协议和https协议。

这段代码最好放到网页尾部。如果需要支持IE 6/7/8，就使用jQuery 1.x版，否则使用最新版。.

## jQuery基础

### jQuery对象

jQuery最重要的概念，就是jQuery对象。它是一个全局对象，可以简写为美元符号$。也就是说，jQuery和$两者是等价的。

在网页中加载jQuery函数库以后，就可以使用jQuery对象了。jQuery的全部方法，都定义在这个对象上面。

{% highlight javascript %}

var listItems = jQuery('li');
// or
var listItems = $('li');

{% endhighlight %}

上面两行代码是等价的，表示选中网页中所有的li元素。

### jQuery构造函数

jQuery对象本质上是一个构造函数，主要作用是返回jQuery对象的实例。比如，上面代码表面上是选中li元素，实际上是返回对应于li元素的jQuery实例。因为只有这样，才能在DOM对象上使用jQuery提供的各种方法。

{% highlight javascript %}

$('body').nodeType
// undefined

$('body') instanceof jQuery
// true

{% endhighlight %}

上面代码表示，由于jQuery返回的不是DOM对象，所以没有DOM属性nodeType。它返回的是jQuery对象的实例。 

**（1）CSS选择器作为参数**

jQuery构造函数的参数，主要是CSS选择器，就像上面的那个例子。下面是另外一些CSS选择器的例子。

{% highlight javascript %}

$("*")
$("#lastname")
$(".intro")
$("h1,div,p")
$("p:last")
$("tr:even")
$("p:first-child")
$("p:nth-of-type(2)")
$("div + p")
$("div:has(p)")
$(":empty")
$("[title^='Tom']")

{% endhighlight %}

本书不讲解CSS选择器，请读者参考有关书籍和jQuery文档。

**（2）DOM对象作为参数**

jQuery构造函数的参数，还可以是DOM对象。它也会被转为jQuery对象的实例。

{% highlight javascript %}

$(document.body) instanceof jQuery
// true

{% endhighlight %}

上面代码中，jQuery的参数不是CSS选择器，而是一个DOM对象，返回的依然是jQuery对象的实例。

如果有多个DOM元素要转为jQuery对象的实例，可以把DOM元素放在一个数组里，输入jQuery构造函数。

{% highlight javascript %}

$([document.body, document.head]) 

{% endhighlight %}

**（3）HTML代码作为参数**

如果直接在jQuery构造函数中输入HTML代码，返回的也是jQuery实例。

{% highlight javascript %}

$('<li class="greet">test</lt>')

{% endhighlight %}

上面代码从HTML代码生成了一个jQuery实例，它与从CSS选择器生成的jQuery实例完全一样。唯一的区别就是，它对应的DOM结构不属于当前文档。

上面代码也可以写成下面这样。

{% highlight javascript %}

$( '<li>', {
  html: 'test',
  'class': 'greet'
});

{% endhighlight %}

上面代码中，由于class是javaScript的保留字，所以只能放在引号中。

**（4）第二个参数**

默认情况下，jQuery将文档的根元素（html）作为寻找匹配对象的起点。如果要指定某个网页元素（比如某个div元素）作为寻找的起点，可以将它放在jQuery函数的第二个参数。

{% highlight javascript %}

$('li', someElement);

{% endhighlight %}

上面代码表示，只寻找属于someElement对象下属的li元素。someElement可以是jQuery对象的实例，也可以是DOM对象。

### jQuery构造函数返回的结果集

jQuery的核心思想是“先选中某些网页元素，然后对其进行某种处理”（find something, do something），也就是说，先选择后处理，这是jQuery的基本操作模式。所以，绝大多数jQuery操作都是从选择器开始的，返回一个选中的结果集。

**（1）length属性**

jQuery对象返回的结果集是一个类似数组的对象，包含了所有被选中的网页元素。查看该对象的length属性，可以知道到底选中了多少个结果。

{% highlight javascript %}

if ( $('li').length === 0 ) {
	console.log('不含li元素');
}

{% endhighlight %}

上面代码表示，如果网页没有li元素，则返回对象的length属性等于0。这就是测试有没有选中的标准方法。

所以，如果想知道jQuery有没有选中相应的元素，不能写成下面这样。

{% highlight javascript %}

if ($('div.foo')) { ... }

{% endhighlight %}

因为不管有没有选中，jQuery构造函数总是返回一个实例对象，而对象的布尔值永远是true。使用length属性才是判断有没有选中的正确方法。

{% highlight javascript %}

if ($('div.foo').length) { ... }

{% endhighlight %}

**（2）下标运算符**

jQuery选择器返回的是一个类似数组的对象。但是，使用下标运算符取出的单个对象，并不是jQuery对象的实例，而是一个DOM对象。

{% highlight javascript %}

$('li')[0] instanceof jQuery // false
$('li')[0] instanceof Element // true

{% endhighlight %}

上面代码表示，下标运算符取出的是Element节点的实例。所以，通常使用下标运算符将jQuery实例转回DOM对象。

**（3）is方法**

is方法返回一个布尔值，表示选中的结果是否符合某个条件。这个用来验证的条件，可以是CSS选择器，也可以是一个函数，或者DOM元素和jQuery实例。

{% highlight javascript %}

$('li').is('li') // true

$('li').is($('.item')) 

$('li').is(document.querySelector('li'))

$('li').is(function() {
      return $("strong", this).length === 0;
});

{% endhighlight %}

**（4）get方法**

jQuery实例的get方法是下标运算符的另一种写法。

{% highlight javascript %}

$('li').get(0) instanceof Element // true

{% endhighlight %}

**（5）eq方法**

如果想要在结果集取出一个jQuery对象的实例，不需要取出DOM对象，则使用eq方法，它的参数是实例在结果集中的位置（从0开始）。

{% highlight javascript %}

$('li').eq(0) instanceof jQuery // true

{% endhighlight %}

由于eq方法返回的是jQuery的实例，所以可以在返回结果上使用jQuery实例对象的方法。

**（6）each方法，map方法**

这两个方法用于遍历结果集，对每一个成员进行某种操作。

each方法接受一个函数作为参数，依次处理集合中的每一个元素。

{% highlight javascript %}

$('li').each(function( index, element) {
  $(element).prepend( '<em>' + index + ': </em>' );
});

// <li>Hello</li>
// <li>World</li>
// 变为
// <li><em>0: </em>Hello</li>
// <li><em>1: </em>World</li>

{% endhighlight %}

从上面代码可以看出，作为each方法参数的函数，本身有两个参数，第一个是当前元素在集合中的位置，第二个是当前元素对应的DOM对象。

map方法的用法与each方法完全一样，区别在于each方法没有返回值，只是对每一个元素执行某种操作，而map方法返回一个新的jQuery对象。

{% highlight javascript %}

$("input").map(function (index, element){
    return $(this).val();
})
.get()
.join(", ")

{% endhighlight %}

上面代码表示，将所有input元素依次取出值，然后通过get方法得到一个包含这些值的数组，最后通过数组的join方法返回一个逗号分割的字符串。

**（8）内置循环**

jQuery默认对当前结果集进行循环处理，所以如果直接使用jQuery内置的某种方法，each和map方法是不必要的。

{% highlight javascript %}

$(".class").addClass("highlight");

{% endhighlight %}

上面代码会执行一个内部循环，对每一个选中的元素进行addClass操作。由于这个原因，对上面操作加上each方法是不必要的。

{% highlight javascript %}

$(".class").each(function(index,element){
	 $(element).addClass("highlight");
});

// 或者

$(".class").each(function(){
	$(this).addClass("highlight");
});

{% endhighlight %}

上面代码的each方法，都是没必要使用的。

由于内置循环的存在，从性能考虑，应该尽量减少不必要的操作步骤。

{% highlight javascript %}

$(".class").css("color", "green").css("font-size", "16px");

// 应该写成

$(".class").css({ 
  "color": "green",
  "font-size": "16px"
});

{% endhighlight %}

### 链式操作

jQuery最方便的一点就是，它的大部分方法返回的都是jQuery对象，因此可以链式操作。也就是说，后一个方法可以紧跟着写在前一个方法后面。

{% highlight javascript %}

$('li').click(function (){
    $(this).addClass('clicked');
})
.find('span')
.attr( 'title', 'Hover over me' );

{% endhighlight %}

### $(document).ready()

$(document).ready方法接受一个函数作为参数，将该参数作为document对象的DOMContentLoaded事件的回调函数。也就是说，当页面解析完成（即下载完&lt;/html&gt;标签）以后，在所有外部资源（图片、脚本等）完成加载之前，该函数就会立刻运行。

{% highlight javascript %}

$( document ).ready(function() {
  console.log( 'ready!' );
});

{% endhighlight %}

上面代码表示，一旦页面完成解析，就会运行ready方法指定的函数，在控制台显示“ready!”。

该方法通常作为网页初始化手段使用，jQuery提供了一种简写法，就是直接把回调函数放在jQuery对象中。

{% highlight javascript %}

$(function() {
  console.log( 'ready!' );
});

{% endhighlight %}

上面代码与前一段代码是等价的。

### $.noConflict方法

jQuery使用美元符号（$）指代jQuery对象。某些情况下，其他函数库也会用到美元符号，为了避免冲突，$.noConflict方法允许将美元符号与jQuery脱钩。

{% highlight html %}

<script src="other_lib.js"></script>
<script src="jquery.js"></script>
<script>$.noConflict();</script>

{% endhighlight %}

上面代码就是$.noConflict方法的一般用法。在加载jQuery之后，立即调用该方法，会使得美元符号还给前面一个函数库。这意味着，其后再调用jQuery，只能写成jQuery.methond的形式，而不能用$.method了。

为了避免冲突，可以考虑从一开始就只使用jQuery代替美元符号。

## jQuery实例对象的方法

除了上一节提到的is、get、eq方法，jQuery实例还有许多其他方法。

### 结果集的过滤方法

选择器选出一组符合条件的网页元素以后，jQuery提供了许多方法，可以过滤结果集，返回更准确的目标。

**（1）first方法，last方法**

first方法返回结果集的第一个成员，last方法返回结果集的最后一个成员。

{% highlight javascript %}

$("li").first()

$("li").last()

{% endhighlight %}

**（2）next方法，prev方法**

next方法返回紧邻的下一个同级元素，prev方法返回紧邻的上一个同级元素。

{% highlight javascript %}

$("li").first().next()
$("li").last().prev()

$("li").first().next('.item')
$("li").last().prev('.item')

{% endhighlight %}

如果next方法和prev方法带有参数，表示选择符合该参数的同级元素。

**（3）parent方法，parents方法，children方法**

parent方法返回当前元素的父元素，parents方法返回当前元素的所有上级元素（直到html元素）。

{% highlight javascript %}

$("p").parent()
$("p").parent(".selected")

$("p").parents()
$("p").parents("div")

{% endhighlight %}

children方法返回选中元素的所有子元素。

{% highlight javascript %}

$("div").children()
$("div").children(".selected")

// 等同于

$('div > *')
$('div > .selected')

{% endhighlight %}

上面这三个方法都接受一个选择器作为参数。

**（4）siblings方法，nextAll方法，prevAll方法**

siblings方法返回当前元素的所有同级元素。

{% highlight javascript %}

$('li').first().siblings()
$('li').first().siblings('.item')

{% endhighlight %}

nextAll方法返回当前元素其后的所有同级元素，prevAll方法返回当前元素前面的所有同级元素。

{% highlight javascript %}

$('li').first().nextAll()
$('li').last().prevAll()

{% endhighlight %}

**（5）closest方法，find方法**

closest方法返回当前元素，以及当前元素的所有上级元素之中，第一个符合条件的元素。find方法返回当前元素的所有符合条件的下级元素。

```javascript

$('li').closest('div')
$('div').find('li')

```

上面代码中的find方法，选中所有div元素下面的li元素，等同于$('li', 'div')。由于这样写缩小了搜索范围，所以要优于$('div li')的写法。

**（6）find方法，add方法，addBack方法，end方法**

add方法用于为结果集添加元素。

{% highlight javascript %}

$('li').add('p')

{% endhighlight %}

addBack方法将当前元素加回原始的结果集。

{% highlight javascript %}

$('li').parent().addBack()

{% endhighlight %}

end方法用于返回原始的结果集。

{% highlight javascript %}

$('li').first().end()

{% endhighlight %}

**（7）filter方法，not方法，has方法**

filter方法用于过滤结果集，它可以接受多种类型的参数，只返回与参数一致的结果。

{% highlight javascript %}

// 返回符合CSS选择器的结果
$('li').filter('.item')

// 返回函数返回值为true的结果
$("li").filter(function(index) {
    return index % 2 === 1;
})

// 返回符合特定DOM对象的结果
$("li").filter(document.getElementById("unique"))

// 返回符合特定jQuery实例的结果
$("li").filter($("#unique"))

{% endhighlight %}

not方法的用法与filter方法完全一致，但是返回相反的结果，即过滤掉匹配项。

{% highlight javascript %}

$('li').not('.item')

{% endhighlight %}

has方法与filter方法作用相同，但是只过滤出子元素符合条件的元素。

{% highlight javascript %}

$("li").has("ul")

{% endhighlight %}

上面代码返回具有ul子元素的li元素。

### DOM相关方法

许多方法可以对DOM元素进行处理。

**（1）html方法和text方法**

html方法返回该元素包含的HTML代码，text方法返回该元素包含的文本。

假定网页只含有一个p元素。

{% highlight html %}

<p><em>Hello World!</em></p>

{% endhighlight %}

html方法和text方法的返回结果分别如下。

{% highlight javascript %}

$('p').html()
// <em>Hello World!</em> 

$('p').text()
// Hello World! 

{% endhighlight %}

jQuery的许多方法都是取值器（getter）与赋值器（setter）的合一，即取值和赋值都是同一个方法，不使用参数的时候为取值器，使用参数的时候为赋值器。

上面代码的html方法和text方法都没有参数，就会当作取值器使用，取回结果集的第一个元素所包含的内容。如果对这两个方法提供参数，就是当作赋值器使用，修改结果集所有成员的内容，并返回原来的结果集，以便进行链式操作。

{% highlight javascript %}

$('p').html('<strong>你好</strong>')
// 网页代码变为<p><strong>你好</strong></p> 

$('p').text('你好')
// 网页代码变为<p>你好</p> 

{% endhighlight %}

下面要讲到的jQuery其他许多方法，都采用这种同一个方法既是取值器又是赋值器的模式。

html方法和text方法还可以接受一个函数作为参数，函数的返回值就是网页元素所要包含的新的代码和文本。这个函数接受两个参数，第一个是网页元素在集合中的位置，第二个参数是网页元素原来的代码或文本。

{% highlight javascript %}

$('li').html(function (i, v){
	return (i + ': ' + v);		
})

// <li>Hello</li>
// <li>World</li>
// 变为
// <li>0: Hello</li>
// <li>1: World</li>

{% endhighlight %}

**（2）addClass方法，removeClass方法，toggleClass方法**

addClass方法用于添加一个类，removeClass方法用于移除一个类，toggleClass方法用于折叠一个类（如果无就添加，如果有就移除）。

{% highlight javascript %}

$('li').addClass('special')
$('li').removeClass('special')
$('li').toggleClass('special')

{% endhighlight %}

**（3）css方法**

css方法用于改变CSS设置。

该方法可以作为取值器使用。

{% highlight javascript %}

$('h1').css('fontSize');

{% endhighlight %}

css方法的参数是css属性名。这里需要注意，CSS属性名的CSS写法和DOM写法，两者都可以接受，比如font-size和fontSize都行。

css方法也可以作为赋值器使用。

{% highlight javascript %}

$('li').css('padding-left', '20px')
// 或者
$('li').css({
  'padding-left': '20px'
});

{% endhighlight %}

上面两种形式都可以用于赋值，jQuery赋值器基本上都是如此。

**（4）val方法**

val方法返回结果集第一个元素的值，或者设置当前结果集所有元素的值。

{% highlight javascript %}

$('input[type="text"]').val()

$('input[type="text"]').val('new value')

{% endhighlight %}

**（5）prop方法，attr方法**

首先，这里要区分两种属性。

一种是网页元素的属性，比如a元素的href属性、img元素的src属性。这要使用attr方法读写。

{% highlight javascript %}

// 读取属性值
$('textarea').attr(name)

//写入属性值
$('textarea').attr(name, val)

{% endhighlight %}

另一种是DOM元素的属性，比如tagName、nodeName、nodeType等等。这要使用prop方法读写。

{% highlight javascript %}

// 读取属性值
$('textarea').prop(name)

// 写入属性值
$('textarea').prop(name, val)

{% endhighlight %}

所以，attr方法和prop方法针对的是不同的属性。在英语中，attr是attribute的缩写，prop是property的缩写，中文很难表达出这种差异。有时，attr方法和prop方法对同一个属性会读到不一样的值。比如，网页上有一个单选框。

{% highlight html %}

<input type="checkbox" checked="checked" />

{% endhighlight %}

对于checked属性，attr方法读到的是checked，prop方法读到的是true。

{% highlight javascript %}

$(input[type=checkbox]).attr("checked") // "checked"

$(input[type=checkbox]).prop("checked") // true

{% endhighlight %}

可以看到，attr方法读取的是网页上该属性的值，而prop方法读取的是DOM元素的该属性的值，根据规范，element.checked应该返回一个布尔值。所以，判断单选框是否选中，要使用prop方法。事实上，不管这个单选框是否选中，attr("checked")的返回值都是checked。

{% highlight javascript %}

if ($(elem).prop("checked")) { /*... */ };

// 下面两种方法亦可

if ( elem.checked ) { /*...*/ };
if ( $(elem).is(":checked") ) { /*...*/ };

{% endhighlight %}

**（6）removeProp方法，removeAttr方法**

removeProp方法移除某个DOM属性，removeAttr方法移除某个HTML属性。

{% highlight javascript %}

$("a").prop("oldValue",1234).removeProp('oldValue')
$('a').removeAttr("title")

{% endhighlight %}

**（7）data方法**

data方法用于在一个DOM对象上储存数据。

{% highlight javascript %}

// 储存数据
$("body").data("foo", 52);

// 读取数据
$("body").data("foo");

{% endhighlight %}

该方法可以在DOM节点上储存各种类型的数据。

### 添加、复制和移动网页元素的方法

jQuery方法提供一系列方法，可以改变元素在文档中的位置。

**（1）append方法，appendTo方法**

append方法将参数中的元素插入当前元素的尾部。

{% highlight javascript %}

$("div").append("<p>World</p>")

// <div>Hello </div>
// 变为
// <div>Hello <p>World</p></div>

{% endhighlight %}

appendTo方法将当前元素插入参数中的元素尾部。

{% highlight javascript %}

$("<p>World</p>").appendTo("div")

{% endhighlight %}

上面代码返回与前一个例子一样的结果。

**（2）prepend方法，prependTo方法**

prepend方法将参数中的元素，变为当前元素的第一个子元素。

{% highlight javascript %}

$("p").prepend("Hello ")

// <p>World</p>
// 变为
// <p>Hello World</p>

{% endhighlight %}

如果prepend方法的参数不是新生成的元素，而是当前页面已存在的元素，则会产生移动元素的效果。

{% highlight javascript %}

$("p").prepend("strong")

// <strong>Hello </strong><p>World</p>
// 变为
// <p><strong>Hello </strong>World</p>

{% endhighlight %}

上面代码运行后，strong元素的位置将发生移动，而不是克隆一个新的strong元素。不过，如果当前结果集包含多个元素，则除了第一个以后，后面的p元素都将插入一个克隆的strong子元素。

prependTo方法将当前元素变为参数中的元素的第一个子元素。

{% highlight javascript %}

$("<p></p>").prependTo("div")

// <div></div>
// 变为
// <div><p></p></div>

{% endhighlight %}

**（3）after方法，insertAfter方法**

after方法将参数中的元素插在当前元素后面。

{% highlight javascript %}

$("div").after("<p></p>")

// <div></div>
// 变为
// <div></div><p></p>

{% endhighlight %}

insertAfter方法将当前元素插在参数中的元素后面。

{% highlight javascript %}

$("<p></p>").insertAfter("div")

{% endhighlight %}

上面代码返回与前一个例子一样的结果。

**（4）before方法，insertBefore方法**

before方法将参数中的元素插在当前元素的前面。

{% highlight javascript %}

$("div").before("<p></p>")

// <div></div>
// 变为
// <p></p><div></div>

{% endhighlight %}

insertBefore方法将当前元素插在参数中的元素的前面。

{% highlight javascript %}

$("<p></p>").insertBefore("div")

{% endhighlight %}

上面代码返回与前一个例子一样的结果。

**（5）wrap方法，wrapAll方法，unwrap方法，wrapInner方法**

wrap方法将参数中的元素变成当前元素的父元素。

{% highlight javascript %}

$("p").wrap("<div></div>")

// <p></p>
// 变为
// <div><p></p></div>

{% endhighlight %}

wrap方法的参数还可以是一个函数。

{% highlight javascript %}

$("p").wrap(function() {
  return "<div></div>";
})

{% endhighlight %}

上面代码返回与前一个例子一样的结果。

wrapAll方法为结果集的所有元素，添加一个共同的父元素。

{% highlight javascript %}

$("p").wrapAll("<div></div>")

// <p></p><p></p>
// 变为
// <div><p></p><p></p></div>

{% endhighlight %}

unwrap方法移除当前元素的父元素。

{% highlight javascript %}

$("p").unwrap()

// <div><p></p></div>
// 变为
// <p></p>

{% endhighlight %}

wrapInner方法为当前元素的所有子元素，添加一个父元素。

{% highlight javascript %}

$("p").wrapInner('<strong></strong>')

// <p>Hello</p>
// 变为
// <p><strong>Hello</strong></p>

{% endhighlight %}

**（6）clone方法**

clone方法克隆当前元素。

{% highlight javascript %}

var clones = $('li').clone();

{% endhighlight %}

对于那些有id属性的节点，clone方法会连id属性一起克隆。所以，要把克隆的节点插入文档的时候，务必要修改或移除id属性。

**（7）remove方法，detach方法，replaceWith方法**

remove方法移除并返回一个元素，取消该元素上所有事件的绑定。detach方法也是移除并返回一个元素，但是不取消该元素上所有事件的绑定。

{% highlight javascript %}

$('p').remove()
$('p').detach()

{% endhighlight %}

replaceWith方法用参数中的元素，替换并返回当前元素，取消当前元素的所有事件的绑定。

{% highlight javascript %}

$('p').replaceWith('<div></div>')

{% endhighlight %}

### 动画效果方法

jQuery提供一些方法，可以很容易地显示网页动画效果。但是，总体上来说，它们不如CSS动画强大和节省资源，所以应该优先考虑使用CSS动画。

如果将jQuery.fx.off设为true，就可以将所有动画效果关闭，使得网页元素的各种变化一步到位，没有中间过渡的动画效果。

**（1）动画效果的简便方法**

jQuery提供以下一些动画效果方法。

- show：显示当前元素。
- hide：隐藏当前元素。
- toggle：显示或隐藏当前元素。
- fadeIn：将当前元素的不透明度（opacity）逐步提升到100%。
- fadeOut：将当前元素的不透明度逐步降为0%。
- slideDown：以从上向下滑入的方式显示当前元素。
- slideUp：以从下向上滑出的方式隐藏当前元素。
- slideToggle：以垂直滑入或滑出的方式，折叠显示当前元素。

上面这些方法可以不带参数调用，也可以接受毫秒或预定义的关键字作为参数。

{% highlight javascript %}

$('.hidden').show();
$('.hidden').show(300);
$('.hidden').show('slow');

{% endhighlight %}

上面三行代码分别表示，以默认速度、300毫秒、较慢的速度隐藏一个元素。

jQuery预定义的关键字是在jQuery.fx.speeds对象上面，可以自行改动这些值，或者创造新的值。

{% highlight javascript %}

jQuery.fx.speeds.fast = 50;
jQuery.fx.speeds.slow = 3000;
jQuery.fx.speeds.normal = 1000;

{% endhighlight %}

上面三行代码重新定义fast和slow关键字对应的毫秒数，并且自定义了normal关键字，表示动画持续时间为1500毫秒。

你可以定义自己的关键字。

{% highlight javascript %}

jQuery.fx.speeds.blazing = 30;

// 调用
$('.hidden').show('blazing');

{% endhighlight %}

这些方法还可以接受一个函数，作为第二个参数，表示动画结束后的回调函数。

{% highlight javascript %}

$('p').fadeOut(300, function() {
  $(this).remove();
});

{% endhighlight %}

上面代码表示，p元素以300毫秒的速度淡出，然后调用回调函数，将其从DOM中移除。

**（2）animate方法**

上面这些动画效果方法，实际上都是animate方法的简便写法。在幕后，jQuery都是统一使用animate方法生成各种动画效果。

animate方法接受三个参数。

{% highlight javascript %}

$('div').animate({
    left: '+=50', // 增加50
    opacity: 0.25,
    fontSize: '12px'
  },
  300, // 持续事件
  function() { // 回调函数
     console.log('done!');
  }
);

{% endhighlight %}

上面代码表示，animate方法的第一个参数是一个对象，表示动画结束时相关CSS属性的值，第二个参数是动画持续的毫秒数，第三个参数是动画结束时的回调函数。需要注意的是，第一个参数对象的成员名称，必须与CSS属性名称一致，如果CSS属性名称带有连字号，则需要用“骆驼拼写法”改写。

**（3）stop方法，delay方法**

stop方法表示立即停止执行当前的动画。

{% highlight javascript %}

$("#stop").click(function() {
  $(".block").stop();
});

{% endhighlight %}

上面代码表示，点击按钮后，block元素的动画效果停止。

delay方法接受一个时间参数，表示暂停多少毫秒后继续执行。

{% highlight javascript %}

$("#foo").slideUp(300).delay(800).fadeIn(400)

{% endhighlight %}

上面代码表示，slideUp动画之后，暂停800毫秒，然后继续执行fadeIn动画。

### 其他方法

jQuery还提供一些供特定元素使用的方法。

serialize方法用于将表单元素的值，转为url使用的查询字符串。

{% highlight javascript %}

$( "form" ).on( "submit", function( event ) {
  event.preventDefault();
  console.log( $( this ).serialize() );
});
// single=Single&multiple=Multiple&check=check2&radio=radio1

{% endhighlight %}

serializeArray方法用于将表单元素的值转为数组。

{% highlight javascript %}

$("form").submit(function (event){
  console.log($(this).serializeArray());
  event.preventDefault();
});
//	[
//		{name : 'field1', value : 123},
//		{name : 'field2', value : 'hello world'}
//	]

{% endhighlight %}

## 事件处理

### 事件绑定的简便方法

jQuery提供一系列方法，允许直接为常见事件绑定回调函数。比如，click方法可以为一个元素绑定click事件的回调函数。

{% highlight javascript %}

$('li').click(function (e){
  console.log($(this).text());
});

{% endhighlight %}

上面代码为li元素绑定click事件的回调函数，点击后在控制台显示li元素包含的文本。

这样绑定事件的简便方法有如下一些：

- click
- keydown
- keypress
- keyup
- mouseover
- mouseout
- mouseenter
- mouseleave
- scroll
- focus
- blur
- resize
- hover

如果不带参数调用这些方法，就是触发相应的事件，从而引发回调函数的运行。

{% highlight javascript %}

$('li').click()

{% endhighlight %}

上面代码将触发click事件的回调函数。

需要注意的是，通过这种方法触发回调函数，将不会引发浏览器对该事件的默认行为。比如，对a元素调用click方法，将只触发事先绑定的回调函数，而不会导致浏览器将页面导向href属性指定的网址。

上面列表的最后一个hover方法需要特别说明。它接受两个回调函数作为参数，分别代表mouseenter和mouseleave事件的回调函数。

{% highlight javascript %}

$(selector).hover(handlerIn, handlerOut)

// 等同于

$(selector).mouseenter(handlerIn).mouseleave(handlerOut)

{% endhighlight %}

### on方法，trigger方法，off方法

除了简便方法，jQuery还提供事件处理的通用方法。

**（1）on方法**

on方法是jQuery事件绑定的统一接口。事件绑定的那些简便方法，其实都是on方法的简写形式。

on方法接受两个参数，第一个是事件名称，第二个是回调函数。

{% highlight javascript %}

$('li').on('click', function (e){
  console.log($(this).text());
});

{% endhighlight %}

上面代码为li元素绑定click事件的回调函数。

> 注意，在回调函数内部，this关键字指的是发生该事件的DOM对象。为了使用jQuery提供的方法，必须将DOM对象转为jQuery对象，因此写成$(this)。

on方法允许一次为多个事件指定同样的回调函数。

{% highlight javascript %}

$('input[type="text"]').on('focus blur', function (){
  console.log('focus or blur');
});

{% endhighlight %}

上面代码为文本框的focus和blur事件绑定同一个回调函数。

on方法还可以为当前元素的某一个子元素，添加回调函数。

{% highlight javascript %}

$('ul').on('click', 'li', function (e){
  console.log(this);
});

{% endhighlight %}

上面代码为ul的子元素li绑定click事件的回调函数。采用这种写法时，on方法接受三个参数，子元素选择器作为第二个参数，夹在事件名称和回调函数之间。

这种写法有两个好处。首先，click事件还是在ul元素上触发回调函数，但是会检查event对象的target属性是否为li子元素，如果为true，再调用回调函数。这样就比为li元素一一绑定回调函数，节省了内存空间。其次，这种绑定的回调函数，对于在绑定后生成的li元素依然有效。

on方法还允许向回调函数传入数据。

{% highlight javascript %}

$("ul" ).on("click", {name: "张三"}, function (event){
	console.log(event.data.name);
});

{% endhighlight %}

上面代码在发生click事件之后，会通过event对象的data属性，在控制台打印出所传入的数据（即“张三”）。

**（2）trigger方法**

trigger方法用于触发回调函数，它的参数就是事件的名称。

{% highlight javascript %}

$('li').trigger('click')

{% endhighlight %}

上面代码触发li元素的click事件回调函数。与那些简便方法一样，trigger方法只触发回调函数，而不会引发浏览器的默认行为。

**（3）off方法**

off方法用于移除事件的回调函数。

{% highlight javascript %}

$('li').off('click')

{% endhighlight %}

上面代码移除li元素所有的click事件回调函数。

**（4）事件的名称空间**

同一个事件有时绑定了多个回调函数，这时如果想移除其中的一个回调函数，可以采用“名称空间”的方式，即为每一个回调函数指定一个二级事件名，然后再用off方法移除这个二级事件的回调函数。

{% highlight javascript %}

$('li').on('click.logging', function (){
  console.log('click.logging callback removed');
});

$('li').off('click.logging');

{% endhighlight %}

上面代码为li元素定义了二级事件click.logging的回调函数，click.logging属于click名称空间，当发生click事件时会触发该回调函数。将click.logging作为off方法的参数，就会移除这个回调函数，但是对其他click事件的回调函数没有影响。

trigger方法也适用带名称空间的事件。

{% highlight javascript %}

$('li').trigger('click.logging')

{% endhighlight %}

### event对象

当回调函数被触发后，它们的参数通常是一个事件对象event。

{% highlight javascript %}

$(document).on('click', function (e){
	// ...
});

{% endhighlight %}

上面代码的回调函数的参数e，就代表事件对象event。

event对象有以下属性：

- type：事件类型，比如click。
- which：触发该事件的鼠标按钮或键盘的键。
- target：事件发生的初始对象。
- data：传入事件对象的数据。
- pageX：事件发生时，鼠标位置的水平坐标（相对于页面左上角）。
- pageY：事件发生时，鼠标位置的垂直坐标（相对于页面左上角）。

event对象有以下方法：

- preventDefault：取消浏览器默认行为。
- stopPropagation：阻止事件向上层元素传播。

### 一次性事件

one方法指定一次性的回调函数，即这个函数只能运行一次。这对提交表单很有用。

{% highlight javascript %}

$("#button").one( "click", function() { return false; } );

{% endhighlight %}

one方法本质上是回调函数运行一次，即解除对事件的监听。

{% highlight javascript %}

document.getElementById("#button").addEventListener("click", handler);

function handler(e) {
    e.target.removeEventListener(e.type, arguments.callee);
	return false;
}

{% endhighlight %}

上面的代码在点击一次以后，取消了对click事件的监听。如果有特殊需要，可以设定点击2次或3次之后取消监听，这都是可以的。

## 参考链接

- Elijah Manor, [Do You Know When You Are Looping in jQuery?](http://www.elijahmanor.com/2013/01/yo-dawg-i-herd-you-like-loops-so-jquery.html)
- Craig Buckler, [How to Create One-Time Events in JavaScript](http://www.sitepoint.com/create-one-time-events-javascript/)
- jQuery Fundamentals, [jQuery Basics](http://jqfundamentals.com/chapter/jquery-basics)
- jQuery Fundamentals, [Animating Your Pages with jQuery](http://jqfundamentals.com/chapter/effects)
