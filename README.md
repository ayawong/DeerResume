DeerResume
==========

![img](http://www.jobdeer.com/img/rd.png)
> [DeerResume][3]项目介绍了如何用markdown制作个人在线简历。不过其中的数据不是存储到本地的，个人感觉不太放心，所以把他的源码clone下来，自己修改了下，可以将数据存储到本地。有需要的同学可以看下。

<!--more-->

# 1.DeerResume简要介绍
下面是[DeerResume][3]官方GitHub的介绍，只是摘抄过来。
最好用的MarkDown在线简历工具，可在线预览、编辑、设置访问密码和生成PDF

  - 可自行搭建，任意修改页面样式和风格
  - 免安装，可放置于任何支持静态页面的云和服务器（当然包括GitHub)
  - 在线MarkDown编辑器+实时预览
  - 在浏览器中实时保存草稿
  - 支持阅读密码，您可以直接将网址和密码发送，供招聘方在线浏览
  - 一键生成简单雅致的PDF，供邮件发送及打印

 点击访问[原项目地址][3]


# 2.个人修改
[DeerResume][3]是在线简历制作工具，只要了解markdown语法就可以写出自己的个性化简历。不过按照[DeerResume][3]介绍的“如何在没有云端的情况下使用DeerResume？”方法并未好使，主要是用到后台PHP请求了，但并没有PHP服务器支持，所以就不好使了。然后我把代码clone下来，研究了一下，对其中有写修改。

主要修改如下：
- 可以将简历数据存储到本地。
- 支持本地设置阅读密码功能。
- 新增简历页面置顶功能。

暂不支持的功能
- 不能在线MarkDown编辑器+实时预览

[此项目示例](http://resumedemo.wiliam.me/)。
初始密码:12345

# 3.FAQ

1. 如何修改访问密码？
`pwd.json`中`vpass`字段存储的是访问密码

2. 如何在显示输入密码的时候显示首页的标题和子标题？
将data.json、err.json和pwd.json中的title字段和subtitle字段都写值即可。

3. 如何编写自己的在线简历？
编写完自己的markdown简历后，将内容复制到data.json中的content字段即可。

4. 修改访问密码后密码不好使？
此问题主要是浏览器的缓存在作怪,修改如下：
修改j`s/app.js`文件的第6行`var pwdurl = 'pwd.json?v=1.0.0';`
将后面的版本号随便修改一个数字；修改index.html文件的28行`<script src="js/app.js?v=1.0.0"></script>`将后面的版本号随便修改一个数字，这时再刷新下浏览器就好使了。


**注意**
这里复制自己的markdown简历到`data.json`中的`content`字段时要注意简历中的换行符。需要替换成\r\n在一行显示才可以。
这里我是自己写java代码替换的，需要使用的有`json-lib`包和apache的`common-io`包，代码如下：
```
import java.io.File;
import java.io.IOException;
import org.apache.commons.io.FileUtils;
import net.sf.json.JSONObject;

public class GenJson {

	public static void main(String[] args) throws IOException {
		resume();
	}
	private static void resume() throws IOException{
		JSONObject jsonObject = new JSONObject();
		jsonObject.put("local", 1);
		jsonObject.put("errno", 0);
		jsonObject.put("show", 1);
		jsonObject.put("title", "Java程序员简历模板");
		jsonObject.put("subtitle", "-----------就是个模板");
		String content = FileUtils.readFileToString
		    (new File("G:/test/jsontest.txt"), "UTF-8");
		jsonObject.put("content", content);
		System.out.println(jsonObject.toString());
	}
	
}      
```

maven构建项目的pom.xml的依赖如下：
```
<dependency>
	<groupId>net.sf.json-lib</groupId>
	<artifactId>json-lib</artifactId>
	<version>2.4</version>
	<classifier>jdk15</classifier><!--指定jdk版本-->  
</dependency>
		
<dependency>
	<groupId>commons-io</groupId>
	<artifactId>commons-io</artifactId>
	<version>2.4</version>
</dependency>

```

示例截图：
![][1]
![][2]

[1]:http://ofv7c2awe.bkt.clouddn.com/mima.jpg
[2]:http://ofv7c2awe.bkt.clouddn.com/DeerResumeSimple.jpg
[3]:https://github.com/geekcompany/DeerResume
