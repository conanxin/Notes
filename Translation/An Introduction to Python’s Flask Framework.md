###设置项目结构
添加文件夹和文件如下：

	├── app
	│   ├── static
	│   │   ├── css
	│   │   ├── img
	│   │   └── js
	│   ├── templates
	│   ├── routes.py
	│   └── README.md

1. 用户请求一个根地址 URL / 进入到主页
2. routes.py映射 URL / 给Python函数
3. Python函数查询在 templates/ 文件夹中的模版
4. 这个模版里有需要的images, CSS, 或JavaScript来渲染HTML
5. 渲染后的HTML返回到routes.py
6. routes.py将HTML发送给浏览器

###创建主页
第一步是定义HTML文档 layout.html，然后放在templates/ 文件夹中：（app/templates/layout.html）
	
	<!DOCTYPE html>
	<html>
  	<head>
    	<title>Flask App</title>    
  	</head>
  	<body>
  
    	<header>
    	  <div class="container">
    	    <h1 class="logo">Flask App</h1>
    	  </div>
    	</header> 
    
    	<div class="container">
    	  {% block content %}
    	  {% endblock %}
    	</div>
    
  	</body>
	</html>

关于{% block content %}{% endblock %}部分，我们又创建了令一个文件home.html：（app/templates/home.html）

	{% extends "layout.html" %}
	{% block content %}
  	<div class="jumbo">
    	<h2>Welcome to the Flask app<h2>
    	<h3>This is the home page for the Flask app<h3>
  	</div>
	{% endblock %}

文件 layout.html定义了一个空块，名字是content，里面有一个子模版。文件home.html是继承自layout.html的子模版，在空块content中填入自己的内容。换句话说，layout.html定义了网站的所有的通用的内容，然后每一个子模版定义自己的内容。

现在我们要映射一个URL，然后我们在浏览器中可以看到。定义roytes.py：（app/routes.py）

	from flask import Flask, render_template

	app = Flask(__name__)      

	@app.route('/')
	def home():
  		return render_template('home.html')

	if __name__ == '__main__':
  		app.run(debug=True)

在routes.py中：

1. 首先，我们导入Flask类和一个render_template函数。
2. 创建一个Flask类的实例。
3. 然后，映射URL /到函数home()。现在我们访问这个地址时，函数home()会执行。
4. 函数home()使用Flask函数render_template()来渲染home.html模版。
5. 最后我们用run()在本地服务器上运行我们的app。我们设置debug为true。

测试：python routes.py

在浏览器打开：http://localhost:5000/

当访问http://localhost:5000/，routes.py会映射URL/到Python函数home()。home()在templates/文件夹中找到模版home.html，渲染为HTML，然后发送给浏览器。

添加CSS，创建文件main.css，在文件夹static/css/中，static/css/main.css：

	body {
	  margin: 0;
	  padding: 0;
	  font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;
	  color: #444;
	}

	/*
	 * Create dark grey header with a white logo
	 */
 
	header {
	  background-color: #2B2B2B;
	  height: 35px;
	  width: 100%;
	  opacity: .9;
	  margin-bottom: 10px;
	}

	header h1.logo {
	  margin: 0;
	  font-size: 1.7em;
	  color: #fff;
	  text-transform: uppercase;
	  float: left;
	}

	header h1.logo:hover {
	  color: #fff;
	  text-decoration: none;
	}

	/*
	 * Center the body content
	 */
 
	.container {
	  width: 940px;
	  margin: 0 auto;
	}

	div.jumbo {
	  padding: 10px 0 30px 0;
	  background-color: #eeeeee;
	  -webkit-border-radius: 6px;
	     -moz-border-radius: 6px;
	          border-radius: 6px;
	}

	h2 {
	  font-size: 3em;
	  margin-top: 40px;
	  text-align: center;
	  letter-spacing: -2px;
	}

	h3 {
	  font-size: 1.7em;
	  font-weight: 100;
	  margin-top: 30px;
	  text-align: center;
	  letter-spacing: -1px;
	  color: #999;
	}

将样式表添加到文件layout.html中，那么通过添加下面代码，这个样式就能应用到所有的子模版：

	 <link rel="stylesheet" href="{{ url_for('static', filename='css/main.css') }}">;

我们使用Flask函数，url_for来生成URL路径给main.css。现在layout.html看起来如下：

	<!DOCTYPE html>
	<html>
  	<head>
    	<title>Flask App</title>    
		<strong><link rel="stylesheet" href="{{ url_for('static', filename='css/main.css') }}"></strong>
  	</head>
  	<body>
  
    	<header>
    	  <div class="container">
    	    <h1 class="logo">Flask App</h1>
    	  </div>
    	</header> 
    
    	<div class="container">
    	  {% block content %}
    	  {% endblock %}
    	</div>
    
  	</body>
	</html>

###创建一个About页面
之前通过扩展layout.html，我们创建了模版home.html，然后我们在routes.py中映射URL/ 给home.html，之后添加样式表使它看起来更好一些。现在我们创建一个about页面。

我们创建一个web模版，about.html，放在templates/文件夹：app/templates/about.html

	{% extends "layout.html" %}
 
	{% block content %}
	  <h2>About</h2>
	  <p>This is an About page for the Intro to Flask article. Don't I look good? Oh stop, you're making me blush.</p>
	{% endblock %}

和之前的home.html一样，我们扩展了layout.html，在content块中填入我们的内容。

打开routes.py，添加映射：

	from flask import Flask, render_template
 
	app = Flask(__name__)
 
	@app.route('/')
	def home():
	  return render_template('home.html')
 
	@app.route('/about')
	def about():
	  return render_template('about.html')
 
	if __name__ == '__main__':
	  app.run(debug=True)

我们映射URL /about给函数about()。打开浏览器http://localhost:5000/about，检查新创建的页面。

###添加导航
打开layout.html，添加链接，在子模版中也能看到，添加<nav>元素在<hearder>元素中：app/templates/layout.html

	...
	<header>
	  <div class="container">
	    <h1 class="logo">Flask App</h1>
	    <strong><nav>
	      <ul class="menu">
	        <li><a href="{{ url_for('home') }}">Home</a></li>
	        <li><a href="{{ url_for('about') }}">About</a></li>
	      </ul>
	    </nav></strong>
	  </div>
	</header>
	...

我们使用Flask函数url_for来生成URLs.

接下来添加更多的规则给main.css，使新添加的导航元素看起来不错：app/static/css/main.css

	...
	/*
	 * Display navigation links inline
	 */

	.menu {
	  float: right;
	  margin-top: 8px;
	}

	.menu li {
	  display: inline;
	}

	.menu li + li {
	  margin-left: 35px;
	}

	.menu li a {
	  color: #999;
	  text-decoration: none;
	}

###结束
现在你完成了一个简单的应用。