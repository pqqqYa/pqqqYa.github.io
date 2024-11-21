---
layout:     post
title:      "Selenium syntax (Java)"
subtitle:   "Selenium语法 (Java)"
date:       2024-11-21 21:29:00
author:     "pqqqYa"
header-img: img/post/2024-11-21-Selenium-syntax-Java-bg.jpg
catalog: true
tags:
    - Selenium
    - Java
    - SoftwareTest
---

# 第零节 环境配置

推荐一个Youtube上的一个教程[How to create Selenium project in IntelliJ IDEA || Ganesh Jadhav](https://youtu.be/cGZf8j6RIRU?si=Umikh0Q_BCp7dK6Y)

首先需要的是Selenium相关的依赖文件的导入和配置，Selenium可以使用python、Java等等语言操作，可以详见[Selenium.dev-Downloads](https://www.selenium.dev/downloads/)，选择自己熟悉的语言，以下就是以Java为例子。

再一个是下载相关浏览器的驱动文件，下文以Microsoft Edge浏览器为例，这里是[官方下载链接](https://developer.microsoft.com/en-us/microsoft-edge/tools/webdriver/?form=MA13LH)。请注意，无论是使用何种浏览器，都需要下载了浏览器对应版本的驱动程序才可以运行。下载完成后，使用如`System.setProperty("webdriver.edge.driver","C:\\Users\\username\\Documents\\msedgedriver.exe");`引入驱动即可使用。


到此为止，我们已经完成了Selenium的环境配置，接下来就可以开始编写我们的第一个软件测试程序了。


# 第一节 八大元素定位

- **为什么要进行元素定位？**

  ​ 我们必须告诉 selenium 怎么去定位元素，用来模拟用户的动作，或者查看元素的属性和状态，以便于我们可以执行检查。例如：我们要搜索一个产品，首先要找到搜索框与搜索按钮，接着通过键盘输入要查询的关键字，最后鼠标单击搜索按钮，提交搜索请求。

  ​ 正如上述的人工操作步骤一样，我们也希望 selenium 能模拟这样的动作，然而，selenium 并不能理解类似在搜索框中输入关键字或者点击搜索按钮这样的图形化的操作。所以需要我们程序化的告诉 selenium 如何定位搜索框和搜索按钮，从而模拟键盘和鼠标的操作。


## 1.1.定位方式

**selenium 提供了8种的定位方式：**

- id
- name
- class name
- tag name
- link text
- partial link text
- xpath
- css selector

这8种定位方式在java selenium 中对应的方法为：

|方法|描述|参数|示例|
|---|---|---|---|
|findElement(By.id())|通过元素的 id 属性值来定位元素|对应的id属性值|findElement(By.id("kw"))|
|findElement(By.name())|通过元素的 name 属性值来定位元素|对应的name值|findElement(By.name("user"))|
|findElement(By.className())|通过元素的 class 名来定位元素|对应的class类名|findElement(By.className("passworld"))|
|findElement(By.tagName())|通过元素的 tag 标签名来定位元素|对应的标签名|findElement(By.tagName("input"))|
|findElement(By.linkText())|通过元素标签对之间的文本信息来定位元素|文本内容|findElement(By.linkText("登录"))|
|findElement(By.partialLinkText())|通过元素标签对之间的部分文本信息来定位元素|部分文本内容|findElement(By.partialLinkText("百度"))|
|findElement(By.xpath())|通过xpath语法来定位元素|xpath表达式|findElement(By.xpath("//input[@id='kw']"))|
|findElement(By.cssSelector())|通过css选择器来定位元素|css元素选择器|findElement(By.cssSelector("#kw"))|

同时这8种方法都对应有着返回复数元素的方法，分别在调用的方法findElements(By.id()) 加上一个s：

- findElements(By.id())
- findElements(By.name())
- findElements(By.className())
- findElements(By.tagName())
- findElements(By.linkText())
- findElements(By.partialLinkText())
- findElements(By.xpath())
- findElements(By.cssSelector())

## 1.2.定位方式的用法

假如我们有一个Web页面，通过前端工具（如，Firebug）查看到一个元素的属性是这样的。


~~~html
<html> 
<head> 
<body link="#0000cc"> 
<a id="result_logo" href="/" onmousedown="return c({'fm':'tab','tab':'logo'})"> 
<form id="form" class="fm" name="f" action="/s"> 
<span class="soutu-btn">按钮</span> 
<input id="kw" class="s_ipt" name="wd" value="" maxlength="255" autocomplete="off">
~~~
我们的目的是要定位input标签的输入框。

- 通过id定位:`driver.findElement(By.id("kw"))`
- 通过name定位:`driver.findElement(By.name("wd"))`
- 通过class name定位:`driver.findElement(By.className("s_ipt"))`
- 通过tag name定位:`driver.findElement(By.tagName("input"))`
- 通过xpath定位，xpath定位有N种写法，这里列几个常用写法:
~~~java
driver.findElement(By.xpath("//*[@id='kw']")) // id定位 
driver.findElement(By.xpath("//*[@name='wd']")) // 属性值定位 
driver.findElement(By.xpath("//span[text()='按钮']")) // 文本定位 
driver.findElement(By.xpath("//input[@class='s_ipt']")) // class属性定位 
driver.findElement(By.xpath("/html/body/form/span/input")) // 绝对路径定位 
driver.findElement(By.xpath("//span[@class='soutu-btn']/input")) // 相对路径定位 
driver.findElement(By.xpath("//form[@id='form']/span/input")) 
driver.findElement(By.xpath("//input[@id='kw' and @name='wd']")) // 多组合属性定位 
driver.findElement(By.xpath("//span[contains(text(),'按钮')]")) // 是否包含文本
~~~
- 通过css定位，css定位有N种写法，这里列几个常用写法:
~~~java
driver.findElement(By.cssSelector("#kw") // id定位 
driver.findElement(By.cssSelector("[name=wd]") // name属性值定位 
driver.findElement(By.cssSelector(".s_ipt") // class地位 
driver.findElement(By.cssSelector("html > body > form > span > input") // css层级定位 
driver.findElement(By.cssSelector("span.soutu-btn> input#kw") 
driver.findElement(By.cssSelector("form#form > span > input")
~~~
假设页面上有一组文本链接。
~~~html
<a class="mnav" href="http://news.baidu.com" name="tj_trnews">新闻</a> 
<a class="mnav" href="http://www.hao123.com" name="tj_trhao123">hao123</a>
~~~

- 通过linkText定位:
~~~java
driver.findElement(By.linkText("新闻") 
driver.findElement(By.linkText("hao123")
~~~
- 通过 partialLinkText 定位:
~~~java
driver.findElement(By.partialLinkText("新") driver.findElement(By.partialLinkText("hao") driver.findElement(By.partialLinkText("123")
~~~
## 1.3.xpath进阶-轴定位

- parent::div 上层父节点，你那叫div的亲生爸爸，最多有一个；
- child::div 下层所有子节点，你的所有亲儿子中叫div的；
- ancestor::div 上面所有直系节点，是你亲生爸爸或者你亲爹或者你亲爹的爸爸中叫div的；
- descendant::div 下面所有节点，你的后代中叫div的，不包括你弟弟的后代；
- following::div 自你以下页面中所有节点叫div的；
- following-sibling::div 同层下节点，你所有的亲弟弟中叫div的；
- preceding::div 同层上节点，你所有的亲哥哥以及他们的后代中叫div的；
- preceding-sibling::div 同层上节点，你所有的亲哥哥中叫div的；

# 第二节 Selenium API

## 2.1.WebDriver 常用 API

WebDriver 提供了一系列的 API 来和浏览器进行交互，如下：

|方法|描述|
|---|---|
|get(String url）|访问目标 url 地址，打开网页|
|getCurrentUrl()|获取当前页面 url 地址|
|getTitle()|获取页面标题|
|getPageSource()|获取页面源代码|
|close()|关闭浏览器当前打开的窗口|
|quit()|关闭浏览器所有的窗口|
|findElement(by)|查找单个元素|
|findElements(by)|查到元素列表，返回一个集合|
|getWindowHandle()|获取当前窗口句柄|
|getWindowHandles()|获取所有窗口的句柄|

## 2.2.WebElement 常用 API

​ 通过 WebElement 实现与网站页面上元素的交互，这些元素包含文本框、文本域、按钮、单选框、div等，WebElement提供了一系列的方法对这些元素进行操作：

|方法|描述|
|---|---|
|click()|对元素进行点击|
|clear()|清空内容（如文本框内容）|
|sendKeys(...)|写入内容与模拟按键操作|
|isDisplayed()|元素是否可见（true:可见，false：不可见）|
|isEnabled()|元素是否启用|
|isSelected()|元素是否已选择|
|getTagName()|获取元素标签名|
|getAttribute(attributeName)|获取元素对应的属性值|
|getText()|获取元素文本值（元素可见状态下才能获取到）|
|submit()|表单提交|
## 2.3 代码演示

~~~java
public class BaiduSearch { 
	public static void main(String[] args) { 
	// 1.创建webdriver驱动 WebDriver 
	driver = new ChromeDriver(); 
	// 2.打开百度首页 
	driver.get("https://www.baidu.com"); 
	// 获取搜索框元素 
	WebElement inputElem = driver.findElement(By.id("kw")); 
	// clear()方法，清空输入框内容 
	inputElem.clear(); 
	// sendKeys()方法，在搜索框中输入搜索内容 
	inputElem.sendKeys("selenium"); 
	// 元素是否显示 
	boolean displayed = inputElem.isDisplayed(); 
	System.out.println(displayed); // 输出true 
	// 元素是否启用 
	boolean enabled = inputElem.isEnabled(); System.out.println(enabled); // 输出true 
	// 判断元素是否被选中状态，一般用在Radio(单选),Checkbox（多选）,Select（下拉选） 
	// 在输入框中使用无意义 
	boolean selected = inputElem.isSelected(); System.out.println(selected); // 输出fasle 
	// 获取标签名 
	String tagName = inputElem.getTagName(); 
	System.out.println(tagName); // 输出input 
	// 获取属性名(name属性) 
	String name = inputElem.getAttribute("name"); 
	System.out.println(name); // 输出wd 
	// 获取文本值 
	String text = inputElem.getText(); 
	System.out.println(text); // 输出selenium 
	// 通过submit提交 
	driver.findElement(By.id("su")).submit(); 
	// click()方法，点击百度一下按钮 
	driver.findElement(By.id("su")).click(); 
	// 退出浏览器 
	driver.quit(); } }
~~~

# 第三节 元素等待机制
​ 在对元素进行定位时，有时候网页加载时间比较长，元素还没有加载出来，这个时候去查找这个元素的话程序中就会抛出异常，所以我们在编写代码时需要考虑延时问题，在selenium中有几种延时机制可以使用如下：

## 3.1.硬性等待

​ 硬性等待就是不管你浏览器元素是否加载完成，都要进行等待设置好的时间，利用 java 语言中的线程类 Thread 中的 sleep 方法，进行强制等待。

`Thread.sleep(long millis) 该方法会让线程进行休眠。`

如：Thread.sleep(3000) 表示程序执行的线程暂停 3 秒钟。

​ 这种方法在一定的程度上是可以解决元素加载过慢的情况，但是不建议使用该方法，因为一般情况下我们无法判断网页到底需要多长时间加载完成，如果我们设置的时间过长，非常影响效率。

## 3.2.隐式等待

​ 隐式等待的理解，就是我们通过代码设置一个等待时间，如果在这个等待时间内，网页加载完成后就执行下一步，否则一直等待到时间截止。

`driver.manage.timeouts.implicitlyWait(long time, TimeUtil unit);`

​ 这种方法相对于硬性等待显的会灵活一点，但是隐式等待也有个弊端，因为这个设置是全局的，程序需要等待整个页面加载完成，直到超时，有时候我需要找的那个元素早就加载完成了，只是页面上有个别其他元素加载比较慢，程序还是会一直等待下去。直到所有的元素加载完成在执行下一步。

## 3.3.显式等待

​ 显示等待是等待指定元素设置的等待时间，在设置时间内，默认每隔0.5s检测一次当前的页面这个元素是否存在，如果在规定的时间内找到了元素则执行相关操作，如果超过设置时间检测不到则抛出异常。默认抛出异常为：NoSuchElementException。推荐使用显示等待。

~~~java
WebDriberWait wait = new WebDriverWait(dirver, timeOutInSeconds); 
wait.nutil(expectCondition);
~~~

1. 查找元素是否已经加载出来
~~~java
WebDriverWait wait = new WebDriverWait(driver, 5); 
// 查找id为“kw"的元素是否加载出来了（已经在页面DOM中存在） 
wait.until(ExpectedConditions.presenceOfElementLocated(By.id("kw")));  
// 在设定时间内找到后就返回，超时直接抛异常`
~~~
2. 查找元素是否可见
~~~java
WebDriverWait wait = new WebDriverWait(driver, 5); 
// 查找id为"kw"的元素是否可见 
wait.until(ExpectedConditions.visibilityOfElementLocated(By.id("kw")));
~~~
3. 查找元素是否可点击
~~~java
WebDriverWait wait = new WebDriverWait(driver, 5); 
// 查找id为"kw"的元素是否可以点击 
wait.until(ExpectedConditions.elementToBeClickable(By.id("kw")));
~~~
4. 自定义方法，重写ExpectedCondition中的apply方法
~~~java
/* 自定义查找元素的方法，对元素查找方法进行二次封装，更加的灵活，可以加上自己逻辑。 */ 
public WebElement getElement(long timeOutInSecond, By by) {         
	WebDriverWait wait = new WebDriverWait(driver, timeOutInSecond);         
	WebElement element = wait.until(new ExpectedCondition<WebElement>() {         
		@NullableDecl             
		@Override             
		public WebElement apply(@NullableDecl WebDriver webDriver) {
			return webDriver.findElement(by);             
		}         
	});                  
return element;     
}
~~~
#### 3.3.1.ExpectedConditions类中常用方法

|方法|描述|
|---|---|
|presenceOfElementLocated(By locator)|判断某个元素是否被加到了dom树里，并不代表该元素一定可见；|
|visibilityOfElementLocated(By locator)|判断某个元素是否可见（代表元素非隐藏，元素的宽和高都不等于0）；|
|elementToBeClickable(By locator)|判断某个元素中是否可见并且是enable的且可点击；|
|elementToBeSelected(By locator)|判断某个元素是否被选中了,一般用在下拉列表；|
|alertIsPresent()|判断页面上是否存在alert；|
|titleIs(String title)|判断当前页面的title是否精确等于预期；|
|titleContains(String title)|判断当前页面的title是否包含预期字符串；|
|textToBePresentInElement(By locator, String text)|判断某个元素中的text是否包含了预期的字符串；|
|textToBePresentInElementValue(By locator, String text)|判断某个元素中的value属性是否包含了预期的字符串；|
|invisibilityOfElementLocated(By locator)|判断某个元素中是否不存在于dom树或不可见；|
|frameToBeAvailableAndSwitchToIt(By)|判断iframe可用，并且切换到iframe中|

## 3.4.页面加载超时设置

通过TimeOuts 对象进行全局页面加载超时的设置，该设置必须放置get 方法之前。如下代码：

~~~java
driver.manage().timeouts().pageLoadTimeout(5, TimeUnit.SECONDS); 
driver.get("https://www.baidu.com");
~~~

如果百度首页在超过5秒钟没有加载完毕，程序就会抛出异常，如果在 2秒就加载完了，就直接往下执行，如果需要对页面加载时间有要求的，可以用这个设置进行检验。

# 第四节 特殊元素操作

## 4.1.弹出框处理（alert、confirm）

操作alert、confirm弹出框，可以通过Alert 对象来进行操作，Alert类包含了确认、取消、输入和获取弹出窗内容。

Alert对应属性和方法：

|方法|描述|
|---|---|
|Alert.getText()|获取弹出框内容。|
|Alert.accept()|接受弹窗的提示，相当于点击确认按钮。|
|Alert.dismiss()|取消提示窗。|
|Alert.sendKeys(String s)|给弹窗输入内容。|

简单使用示例：

~~~java
// 首先需要切换到弹出框中，获取Alert对象。 
Alert alert = driver.switchTo().alert(); 
// 获取弹窗文本内容 
alert.getText(); 
// 点击确定按钮 
alert.accept(); 
// 点击取消按钮 
alert.dismiss();
~~~


注：如果弹出框不是 js 原生的 alert 弹窗，我们还是按照原来的获取元素的方法。

## 4.2.iframe 切换

有时候我们定位元素的时候，发现怎么都定位不了。 这时候你需要查一查你要定位的元素是否在iframe里面。

**什么是iframe？**

iframe 就是HTML 中，用于网页嵌套网页的。 一个网页可以嵌套到另一个网页中，可以嵌套很多层。

例如：

**main.html**
~~~html
<html> 
<head>   
	<title>FrameTest</title> 
</head> 
<body>   
	<div id="id1">this is main page's div!</div>   
	<input type="text" id="maininput" />   
	<br/>   
	<iframe id="frameA" frameborder="0" scrolling="no" style="left:0;position:absolute;" src="frame.html"></iframe> 
</body> 
</html>
~~~


**frame.html**

~~~html
<html> 
<head>   
	<title>this is a frame!</title> 
</head> 
<body>   
	<div id="div1">this is iframes div！</div>   
	<input id="iframeinput"></input> 
</body> 
</html>
~~~

使用selenium 操作浏览器时，如果需要操作iframe中的元素，首先需要切换到对应的内联框架中。

selenium 给我们提供了三个重载的方法，进行操作iframe；

**切换方法：**

~~~java

// 方法一:通过 iframe的索引值，在页面中的位置 
driver.switchTo().frame(index); 
// 方法二：通过 iframe 的name 或者id 
driver.switchTo().frame(nameOrId); 
// 方法三：通过iframe 对应的webElement         
driver.switchTo().frame(frameElement);
~~~
**selenium 代码:**

~~~java
public static void testIframe(WebDriver driver){     
	// 在 主窗口的时候     
	driver.findElement(By.id("maininput")).sendKeys("main input");     
	// 此时 没有进入到iframe, 以下语句会报错     
	//driver.findElement(By.id("iframeinput")).sendKeys("iframe input");  
	driver.switchTo().frame("frameA");     
	driver.findElement(By.id("iframeinput")).sendKeys("iframe input");      
	// 此时没有在主窗口，下面语句会报错     
	//driver.findElement(By.id("maininput")).sendKeys("main input");      
	
	// 回到主窗口     
	driver.switchTo().defaultContent();     
	driver.findElement(By.id("maininput")).sendKeys("main input");  
}
~~~

注：如果已经切换进入了其中的一个 iframe 中，再想对 iframe 外的元素进行操作，需要切换回到默认的页面中，否则会找不到元素。

~~~java
// 切换到默认内容页面 
driver.switchTo().defaultContent();
~~~


## 4.3.浏览器窗口的切换

​ 有时候后在操作浏览器，可能打开了一个新的窗口，这个时候如果要对新窗口的元素进行操作，需要切换到新窗口中去，怎么去切换呢？在 selenium 中有个叫**句柄**的概念。

​ 什么是**句柄**，简单理解就是浏览器窗口的一个标识，浏览器打开的每个窗口都有唯一的一个标识，也就是句柄，我们可以通过句柄来进行窗口之间的切换，从而来达到我们操作不同窗口的元素。

WebDriver 中提供了两个 API 来获取窗口的相关句柄：

~~~java
// 获取当前窗口的句柄 
String handle = driver.getWindowHandle(); 
// 获取所有窗口的句柄，返回一个集合 
Set<String> handles = driver.getWindowHandles();
~~~
获取到句柄后，通过对应的方法进行切换：

~~~java
// 切换到窗口 
driver.switchTo.windwo(String handle);
~~~
多窗口之间的切换方法：

~~~java
/** * 切换窗口的方法 * 通过传入一个标题来找到我们需要的窗口。 * @param title 窗口的标题 */ 
public void switchWindow(String title){ 
	Set<String> handles = driver.getWindowHandles(); 
	// 切换窗口的方式--循环遍历handles集合 
	for (String handle : handles) { 
	//判断是哪一个页面的句柄？？--根据什么来判断？？？title 
		if(driver.getTitle().equals(title)){ 
		break; 
	}else{ 
		//切换窗口--根据窗口标识来切换 
		driver.switchTo().window(handle); 
	} 
}
~~~
## 4.4.select 下拉框处理

如果一个页面元素是一个下拉框（select），对应下拉框的操作，selenium有专门的类 Select 进行处理。其中包含了单选和多选下拉框的各种操作，如获得所有的选项、选择某一项、取消选中某一项、是否是多选下拉框等。

**Select类常用的一些方法：**

|方法|说明|
|---|---|
|void [deselectAll](http://seleniumhq.github.io/selenium/docs/api/java/org/openqa/selenium/support/ui/Select.html#deselectAll--)()|取消所有选择项，仅对下拉框的多选模式有效，若下拉不支持多选模式，则会抛出异常 UnsupportedOperationException（不支持的操作）|
|void [deselectByIndex](http://seleniumhq.github.io/selenium/docs/api/java/org/openqa/selenium/support/ui/Select.html#deselectByIndex-int-)(int index)|取消指定index的选择，index从零开始，仅对多选模式有效，否则抛出异常 UnsupportedOperationException（不支持的操作）|
|void [deselectByValue](http://seleniumhq.github.io/selenium/docs/api/java/org/openqa/selenium/support/ui/Select.html#deselectByValue-java.lang.String-)(String value)|取消Select标签中，value为指定值的选择，仅对多选模式有效，否则抛出异常 UnsupportedOperationException（不支持的操作）|
|void [deselectByVisibleText](http://seleniumhq.github.io/selenium/docs/api/java/org/openqa/selenium/support/ui/Select.html#deselectByVisibleText-java.lang.String-)(String Text)|取消项的文字为指定值的项，例如指定值为Bar，项的html为，仅对多选模式有效，单选模式无效，但不会抛出异常|
|List`getAllSelectedOptions()`|获得所有选中项，单选多选模式均有效，但没有一个被选中时，返回空列表，不会抛出异常|
|WebElement `getFirstSelectedOption()`|获得第一个被选中的项，单选多选模式均有效，当多选模式下，没有一个被选中时，会抛出[NoSuchElementException](http://seleniumhq.github.io/selenium/docs/api/java/org/openqa/selenium/NoSuchElementException.html)异常|
|List`getOptions()`|获得下拉框的所有项，单选多选模式均有效，当下拉框没有任何项时，返回空列表，不会抛出异常|
|boolean `isMultiple()`|判断下拉框是否多选模式|
|void [selectByIndex](http://seleniumhq.github.io/selenium/docs/api/java/org/openqa/selenium/support/ui/Select.html#selectByIndex-int-)(int index)|选中指定index的项，单选多选均有效，当index超出范围时，抛出[NoSuchElementException](http://seleniumhq.github.io/selenium/docs/api/java/org/openqa/selenium/NoSuchElementException.html)异常|
|void [selectByValue](http://seleniumhq.github.io/selenium/docs/api/java/org/openqa/selenium/support/ui/Select.html#selectByValue-java.lang.String-)(String value)|选中所有Select标签中，value为指定值的所有项，单选多选均有效，当没有适合的项时，抛出[NoSuchElementException](http://seleniumhq.github.io/selenium/docs/api/java/org/openqa/selenium/NoSuchElementException.html)异常|
|void [selectByVisibleText](http://seleniumhq.github.io/selenium/docs/api/java/org/openqa/selenium/support/ui/Select.html#selectByVisibleText-java.lang.String-)(String text)|选中所有项的文字为指定值的项，与deselectByValue相反，但单选多选模式均有效，当没有适合的项时，抛出[NoSuchElementException](http://seleniumhq.github.io/selenium/docs/api/java/org/openqa/selenium/NoSuchElementException.html)异常|

**示例：2345网址导航首页的城市省份切换。**

1. 进入2345.com首页，点击头部【切换】进行城市切换，我们切换省份为北京。

[![](https://pic.downk.cc/item/5e8079f1504f4bcb04461282.png)](https://pic.downk.cc/item/5e8079f1504f4bcb04461282.png)

2. HTML页面DOM结构。

[![](https://pic.downk.cc/item/5e8079f1504f4bcb04461285.png)](https://pic.downk.cc/item/5e8079f1504f4bcb04461285.png)

3. 代码编写，这里需要注意下拉选是在一个iframe中，需要先切换到这个iframe后再操作。

~~~java
// 创建驱动 
WebDriver driver = new ChromeDriver(); 
// 打开2345网站 
driver.get("https://www.2345.com"); 
// 切换城市 
driver.findElement(By.linkText("切换")).click(); 
// 切换到iframe内联框架中 
driver.switchTo().frame("city_set_ifr"); 
// 定位到省份下拉框 
WebElement province = driver.findElement(By.id("province")); 
province.click(); 
// 创建Select对象 
Select select = new Select(province); 
// 根据文本来获取下拉值 
select.selectByVisibleText("B 北京"); 
driver.quit();
~~~

## 4.5.带 readonly 属性的元素操作[#](https://www.cnblogs.com/tester-ggf/p/12602211.html#2745676242)

​ 标签元素如果带有 readonly 属性，表示只读不能进行编辑，如果我们需要操作这样的元素，需要把这个 readonly 属性进行移除后，再进行操作。删除标签属性的话，webdriver 没有对应的 API，我们使用 JavaScript 脚本来进行操作。

**示例：12306 网站购票页面日期。**

[![](https://pic.downk.cc/item/5e80841d504f4bcb044e452b.png)

selenium 代码实现：

~~~java
// 创建驱动 
WebDriver driver = new ChromeDriver(); 
// 打开12306网站 
driver.get("https://www.12306.cn/index/"); 
// 通过js来移除readonly属性 
String removeAttr = "document.getElementById('train_date').removeAttribute('readonly');"; 
// 执行js 
((JavascriptExecutor)driver).executeScript(removeAttr); 
// 获取日期日历输入框 
WebElement train_date = driver.findElement(By.id("train_date")); 
// 清除原来的值 
train_date.clear(); 
// 输入内容 
train_date.sendKeys("2020-03-30"); driver.quit();
~~~

## 4.6.日期控件操作

对于页面中出现时间控件选择时，一般分为两种：

1. 控件没有限制手动填写的，我们直接使用 sendKeys() 方法进行赋值即可。
~~~java
driver.findElement(By).sendKeys("2020-03-30");
~~~
2. 控件限制了手动输入的，只能通过点击控件时间进行输入的，我们就需要使用 js 脚本进行操作了。
~~~java
// 获取js执行器 
JavaScriptExecutor js = (JavaScriptExecutor)driver; 
// 对时间输入框进入赋值 
String script = "document.getElementById('xxx').value='2020-03-30';"; 
// 执行 
js.executeScript(script);
~~~

注：需要注意的是，不管使用哪种方式进行时间的赋值，一点要注意输入时间的格式是否符合系统的要求；

## 4.7.文件上传

对于通过input标签实现的上传功能，可以将其看作是一个输入框，即通过sendKeys()指定本地文件路径的方式实现文件上传。

创建upfile.html文件，代码如下：

~~~html

<html> 
<head> 
	<meta http-equiv="content-type" content="text/html;charset=utf-8" /> 
	<title>upload_file</title> 
	<link href="http://cdn.bootcss.com/bootstrap/3.3.0/css/bootstrap.min.css" rel="stylesheet" /> 
</head> 
<body>   
	<div class="row-fluid">     
		<div class="span6 well">     
		<h3>upload_file</h3>       
			<input type="file" name="file" />     
		</div>   
	</div> 
</body> 
<script src="http://cdn.bootcss.com/bootstrap/3.3.0/css/bootstrap.min.js"></scrip> 
</html>
~~~

接下来通过sendKeys()方法来实现文件上传。

~~~java
import java.io.File; 
import org.openqa.selenium.By; 
import org.openqa.selenium.WebDriver; 
import org.openqa.selenium.chrome.ChromeDriver;  
public class UpFileDemo {    
	public static void main(String[] args) throws InterruptedException {      
		WebDriver driver = new ChromeDriver();     
		File file = new File("./HTMLFile/upfile.html");     
		String filePath = file.getAbsolutePath();     
		driver.get(filePath);      
		
		//定位上传按钮， 添加本地文件     
		driver.findElement(By.name("file")).sendKeys("D:\\upload_file.txt");     
		Thread.sleep(5000);      
		driver.quit();   
	} 
}
~~~

注：sendKeys 参数为文件的绝对路径，并且上传的文件一点要存在，否则会抛异常。

# 第五节 控制浏览器操作

## 5.1.浏览器窗口操作

WebDriver 给我们提供了一个 Window 对象，专门用于对窗口的设置。

对象获取方法：`Window window = driver.manage().window();`

Window 对象的方法有：

|方法|描述|
|---|---|
|window.maximize()|将浏览器窗口最大化。|
|window.getPosition()|获取窗口的位置，返回 Point 对象，包含浏览器左上角的坐标位置。通过point.x 和point.y 来获取到。|
|window.setPosition(Point)|指定浏览器窗口左上角的坐标位置，创建一个Point 对象，设置对象的 x 和 y 坐标即可。|
|window.getSize()|获取窗口尺寸（宽和高），返回一个 Dimension 对象，通过该对象调用 getHeight() 和 getWidth() 来获取 高度和宽度。|
|window.setSize(Dimension)|设置窗口大小，创建一个 Dimension 对象，设置对象的高度和宽度。|

## 5.2.浏览器导航操作

WebDriver 提供了 Navigation 对象来对浏览器进行导航操作，如：前进、后退、刷新等。

Navigation 对象获取：`Navigation navigate = driver.navigate();`

Navigation 对象提供的方法：

|方法|描述|
|---|---|
|navigate.to(url)|跳转到指定url,和 webdriver 使用 get 方法是一样的。|
|navigate.refresh()|刷新当前页面。|
|navigate.back()|浏览器回退操作。|
|navigate.forward()|浏览器前进操作。|

# 第六节 模拟鼠标键盘操作

## 6.1.模拟鼠标

在WebDriver中，关于鼠标的操作我们可以通过 Actions 类来模拟鼠标右击、双击、悬停、拖动等操作。

Actions 类中鼠标操作常用方法：

|方法|描述|
|---|---|
|contextClick()|鼠标右击|
|clickAndHold(WebElement)|点击并控制（模拟悬停）|
|doubleClick(WebElement)|鼠标双击|
|dragAndDrop(webElement1,webElement2)|鼠标拖动|
|moveToElement(WebElement)|鼠标移动到某个元素上|
|perform()|执行所有Actions中存储的行为|
|click()|鼠标单击（左击）|

**示例：百度首页设置悬停下拉菜单**

~~~java
import org.openqa.selenium.By; 
import org.openqa.selenium.WebDriver; 
import org.openqa.selenium.WebElement; 
import org.openqa.selenium.chrome.ChromeDriver; 
import org.openqa.selenium.interactions.Actions;  
public class MouseDemo {    
	public static void main(String[] args) {      
		WebDriver driver = new ChromeDriver();     
		driver.get("https://www.baidu.com/"); 	
		// 定位元素     
		WebElement search_setting = driver.findElement(By.linkText("设置"));     
		// 创建actions对象     
		Actions action = new Actions(driver);     
		// 模拟鼠标悬停     
		action.clickAndHold(search_setting).perform();      
		driver.quit();   
	} 
}
~~~
其他方法使用：

~~~java
Actions action = new Actions(driver);  
// 鼠标右键点击指定的元素 
action.contextClick(driver.findElement(By.id("element"))).perform();  
// 鼠标双击指定的元素 
action.doubleClick(driver.findElement(By.id("element"))).perform(); 
// 鼠标移到到指定元素上 
action.moveToElement(driver.findElement(By.id("element"))).perform();  
// 鼠标拖拽动作， 将 source 元素拖放到 target 元素的位置。 
WebElement source = driver.findElement(By.name("element")); 
WebElement target = driver.findElement(By.name("element")); 
action.dragAndDrop(source,target).perform();  
// 释放鼠标 
action.release().perform();
~~~
## 6.2.模拟键盘

在 selenium 中有个 Keys() 类（枚举类），提供了几乎键盘上所有按键的方法，在使用的过程中，我们可以通过 sendKeys() 方法来模拟键盘的输入，除此之外，我们还可以用它来输入键盘上的按键， 甚至是组合键， 如 Ctrl+A、 Ctrl+C 等。

**以下为常用的键盘操作：**

- sendKeys(Keys.BACK_SPACE) 回格键（BackSpace）
- sendKeys(Keys.SPACE) 空格键 (Space)
- sendKeys(Keys.TAB) 制表键 (Tab)
- sendKeys(Keys.ESCAPE) 回退键（Esc）
- sendKeys(Keys.ENTER) 回车键（Enter）
- sendKeys(Keys.CONTROL,'a') 全选（Ctrl+A）
- sendKeys(Keys.CONTROL,'c') 复制（Ctrl+C）
- sendKeys(Keys.CONTROL,'x') 剪切（Ctrl+X）
- sendKeys(Keys.CONTROL,'v') 粘贴（Ctrl+V）
- sendKeys(Keys.F1) 键盘 F1
- ……
- sendKeys(Keys.F12) 键盘 F12


在使用键盘按键方法前，我们需要先导入 keys 类。

~~~java
import org.openqa.selenium.WebElement; 
import org.openqa.selenium.WebDriver; 
import org.openqa.selenium.chrome.ChromeDriver; 
import org.openqa.selenium.By; 
import org.openqa.selenium.Keys;  
public class Keyboard {    
	public static void main(String[] args)throws InterruptedException {      
		WebDriver driver = new ChromeDriver();     
		driver.get("https://www.baidu.com");      
		// 定位到对应的元素     
		WebElement input = driver.findElement(By.id("kw"));      
		//输入框输入内容     
		input.sendKeys("seleniumm");     
		Thread.sleep(2000);      
		//删除多输入的一个 m     
		input.sendKeys(Keys.BACK_SPACE);     
		Thread.sleep(2000);      
		//输入空格键+“教程”     
		input.sendKeys(Keys.SPACE);     
		input.sendKeys("教程");     
		Thread.sleep(2000);      
		//ctrl+a 全选输入框内容     
		input.sendKeys(Keys.CONTROL,"a");     
		Thread.sleep(2000);      
		//ctrl+x 剪切输入框内容     
		input.sendKeys(Keys.CONTROL,"x");     
		Thread.sleep(2000);      
		//ctrl+v 粘贴内容到输入框     
		input.sendKeys(Keys.CONTROL,"v");     
		Thread.sleep(2000);      
		//通过回车键盘来代替点击操作     
		input.sendKeys(Keys.ENTER);     
		Thread.sleep(2000);      
		driver.quit();   
	} 
}
~~~

记录：在 Actions 类中也有对应操作键盘的方法，例如：keyUp()、keyDown()等，但是我在实际使用中，并没有生效，不知道为何，从网上资料说是，不能直接对浏览器进行操作，只能对页面的元素进行键盘的模拟操作。

# 第七节 操作javaScript代码

虽然WebDriver提供了操作浏览器的前进和后退方法，但对于浏览器滚动条并没有提供相应的操作方法。在这种情况下，就可以借助JavaScript来控制浏览器的滚动条。WebDriver提供了executeScript()方法来执行JavaScript代码。

用于调整浏览器滚动条位置的JavaScript代码如下：`<!-- window.scrollTo(左边距,上边距); --> window.scrollTo(0,450);`

window.scrollTo() 方法用于设置浏览器窗口滚动条的水平和垂直位置。方法的第一个参数表示水平的左间距，第二个参数表示垂直的上边距。其代码如下：

~~~java
import org.openqa.selenium.By; 
import org.openqa.selenium.WebDriver; 
import org.openqa.selenium.Dimension; 
import org.openqa.selenium.chrome.ChromeDriver; 
import org.openqa.selenium.JavascriptExecutor;  
public class JSDemo {    
	public static void main(String[] args) throws InterruptedException{      
		WebDriver driver = new ChromeDriver();      
		//设置浏览器窗口大小     
		driver.manage().window().setSize(new Dimension(700, 600));     
		driver.get("https://www.baidu.com");      
		//进行百度搜索     
		driver.findElement(By.id("kw")).sendKeys("webdriver api");     
		driver.findElement(By.id("su")).click();     
		Thread.sleep(2000);      
		//将页面滚动条拖到底部     
		((JavascriptExecutor)driver).executeScript("window.scrollTo(100,450);");  
		Thread.sleep(3000);      
		driver.quit();   
	} 
}
~~~
通过浏览器打开百度进行搜索，并且提前通过 window().setSize() 方法将浏览器窗口设置为固定宽高显示，目的是让窗口出现水平和垂直滚动条。然后通过 executeScript() 方法执行JavaScripts代码来移动滚动条的位置。

**将滚动条滚动到某个区域后停止(页面元素全部加载完成)，如下：**

~~~java
//滚动到某一区域 
//scrollIntoView(0);  让元素滚动到可视区域的最下方 
//scrollIntoView();  让元素滚动到可视区域的最上方 
//JavascriptExecutor javascriptExecutor = (JavascriptExecutor)BrowserUtil.driver; 
//javascriptExecutor.executeScript("document.getElementById('index_ads').scrollIntoView(0);"); 
//JavaScript的参数传递-selenium和js的交互 
//1、先去找到这个元素 
WebElement webElement = driver.findElement(By.xpath("element")); 
//2、找到的元素作为参数传入到Js代码中 
JavascriptExecutor javascriptExecutor = (JavascriptExecutor)driver; 
javascriptExecutor.executeScript("arguments[0].scrollIntoView(0)",webElement);
~~~

**页面元素是通过懒加载方式，需要一直进行滚动的**

~~~java
/** 
* 滑动列表找元素并且进行点击（懒加载） 
* @param selectedText 选中元素文本 
* @param by 正在加载类似元素的定位表达式 
*/ 
public static void clickElementInList(String selectedText, By by) { 
	// 滑动之前的页面源代码信息 
	String beforeSource = ""; 
	// 滑动之后的页面源代码信息 
	String afterSource = ""; 
	// 循环条件 
	// 1、找到了元素，跳出循环 
	// 2、如果没有找到元素？？？怎么跳出循环 
	while (true) { 
		WebElement webElement = driver.findElement(by); 
		// 获取页面源代码 
		beforeSource = driver.getPageSource(); 
		// 获取js执行器 
		JavascriptExecutor javascriptExecutor = (JavascriptExecutor)driver; 
		// 执行js 
		javascriptExecutor.executeScript("arguments[0].scrollIntoView(0);", webElement); 
		// 如果当前页面有想要的元素，怎么判断是否有？？--getPageSource 
		if (driver.getPageSource().contains(selectedText)) { 
			driver.findElement(By.linkText(selectedText)).click(); 
			// 找到元素退出循环，不再滚动。 
			break; 
		} 
		afterSource = driver.getPageSource();
		 // 页面元素没有变化---滑动到了最底部 
		 if (afterSource.equals(beforeSource)) { 
			 // 到达底部，退出。 
			 break; 
		} 
	}
}
~~~