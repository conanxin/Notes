###创建Bottlle应用

Bottle应用的是MVC模式。MVC表示模型(model)，视图(view)和控制器(controller)。

Model表示一组数据，然后用来存储、查询和更新数据。View表示信息如何呈现给用户。它用来格式化和控制呈现的数据。Controller是应用的处理中心，决定如何相应用户的请求。

###应用MVC设计
**创建Model**

我们使用SQLite数据库作为我们的数据库。

首先要安装sqlite，然后还要安装Bottle插件，使我们可以使用这些数据库：
	
	pip install bottle-sqlite

我们可以创建一个简单的数据库来存储我们的数据。创建一个python脚本（picnic_data.py）来生成一个SQLite数据库，其中还有一些数据，运行这个程序，代码如下：
	
	import sqlite3
	db = sqlite3.connect('picnic.db')
	db.execute("CREATE TABLE picnic (id INTEGER PRIMARY KEY, item CHAR(100) NOT NULL, quant INTEGER NOT NULL)")
	db.execute("INSERT INTO picnic (item,quant) VALUTES ('bread', 4)")
	db.execute("INSERT INTO picnic (item,quant) VALUTES ('cheese', 2)")
	db.execute("INSERT INTO picnic (item,quant) VALUTES ('grapes', 30)")
	db.execute("INSERT INTO picnic (item,quant) VALUTES ('cake', 1)")
	db.execute("INSERT INTO picnic (item,quant) VALUTES ('soda', 4)")
	db.commit()

执行上面的程序，会生成一个数据库文件picnic.db。

**创建Controller**

我们已经有了数据库，开始写主要的程序。创建一个文件picnic.py。

定义和URL地址 _/picnic_ 匹配的route。

	import sqlite3
	from bottle import route, run, template

	@route('/picnic')

实现函数能和数据库连接，从表格中得到我们的数据，然后调用“View”来展示页面。最后返回已经格式化的输出给用户。

	import sqlite3
	from bottle import route, run, template

	@route('/picnic')
	def show_picnic():
	    db = sqlite3.connect('picnic.db')
	    c = db.cursor()
	    c.execute("SELECT task,quant FROM picnic")
	    data = c.fetchall()
	    c.close()
	    output = template('bring_to_picnic' rows=data)
	    return output

最后，我们需要来运行实际的服务器：
	
	import sqlite3
	from bottle import route, run, template

	@route('/picnic')
	def show_picnic():
    	db = sqlite3.connect('picnic.db')
    	c = db.cursor()
    	c.execute("SELECT item,quant FROM picnic")
    	data = c.fetchall()
    	c.close()
    	output = template('bring_to_picnic', rows=data)
    	return output

	run(host='0.0.0.0', port=8080)

我们使用命令 _db = sqlite3.connect('picnic.db')_ 来连接数据库。我们查询数据库，并在接下来四行代码中选择所有的值。

代码 _output = template('bring_to_picnic', rows=data)_ 用来格式化数据。它调用一个称为bring_to_picnic的template（view）来格式化数据。传递"data"变量给template变量"rows"。

**创建View**

现在要创建我们的view,通过Bottle内建的template engine很容易实现。

这个应用会搜索一个template来匹配给定的template function名字，以_.tpl_结尾。这个文件可以在主目录下，或是在"view"文件夹下。

创建一个文件匹配我们在template function中调用的名字，bring_to_picnic.tpl，在这个文件里，我们混合使用HTML和程序。代码很简单，循环创建一个表格，然后填入我们的model data：

	<h1>Things to bring to our picnic</h1>
	<table>
	<tr><th>Item</th><th>Quantity</th></tr>
	%for row in rows:
    	<tr>
    	%for col in row:
        	<td>{{col}}</td>
    	%end
    	</tr>
	%end
	</table>

传递给模版的"rows"变量在描述输出时可用。

我们在"%"中可以输入Python程序，我们可以在HTML中通过使用"{{var}}"来访问变量。

**效果**

运行程序
	
	python picnic.py

