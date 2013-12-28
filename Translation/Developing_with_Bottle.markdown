# 使用Bottle开发

##写app

创建一个文件app.py，程序如下：

    import os
	from bottle import route, run, template

	index_html = '''My first web app! By {{ author }}'''

	@route('/:anything')
	def something(anything=''):
    	return template(index_html, author=anything)

	@route('/')
	def index():
    	return template(index_html, author='your name here')

	if __name__ == '__main__':
    	port = int(os.environ.get('PORT', 8080))
    	run(host='0.0.0.0', port=port, debug=True)

运行：
	python app.py

然后访问：http://localhost:8080/abc，可以将abc替换为你的名字。

##解释
1. _@route_ 是告诉app的路径 _/_ ,变量为 _anything_ 。
2. 作为参数传递给函数 _def something(anything='')_
3. 传递参数给模版函数作为一个参数（ _author=anything_）
4. 这个模版，然后渲染作者变量 _{{author}}_

##下一步
然后可以添加新的 _@route_ 来创建新的页面。

创建这个HTML很简单：在上面的app里，我们可以在文件里面添加HTML代码。

##使用plot
注册一个plot帐号，https://www.plot.ly/api，然后创建一个新的API key。

安装plot.ly：
    pip install plotly

更新app.py代码：
	
	import os
	from bottle import run, template, get, post, request
	from plotly import plotly

	py = plotly(username='mjhea0', key='2ic27cpzex')

	@get('/plot')
	def form():
    	return '''<h2>Graph via Plot.ly</h2>
    	          <form method="POST" action="/plot">
    	            Name: <input name="name1" type="text" />
    	            Age: <input name="age1" type="text" /><br/>
   	             	Name: <input name="name2" type="text" />
    	            Age: <input name="age2" type="text" /><br/>
	                Name: <input name="name3" type="text" />
    	            Age: <input name="age3" type="text" /><br/>                
    	            <input type="submit" />
    	          </form>'''

	@post('/plot')
	def submit():
    	name1   = request.forms.get('name1')
    	age1    = request.forms.get('age1')
    	name2   = request.forms.get('name2')
    	age2    = request.forms.get('age2')
    	name3   = request.forms.get('name3')
    	age3    = request.forms.get('age3')

	    x0 = [name1, name2, name3];
    	y0 = [age1, age2, age3];
    	data = {'x': x0, 'y': y0, 'type': 'bar'}
    	response = py.plot([data])
    	url = response['url']
    	filename = response['filename']
    	return template('''Congrats! View your chart here <a href=""></a>!''', url=url)

	if __name__ == '__main__':
    	port = int(os.environ.get('PORT', 8080))
    	run(host='0.0.0.0', port=port, debug=True)

##介绍
在第一个函数，form()，我们创建一个HTML表格来获取数据制作一个柱状图。然后第二个函数，submit()，我们获取表格输入，赋给变量，然后使用plot.ly的API来生成新的表格。注意替换username和key为你自己的。

运行
	python app.py

然后打开http://localhost:8080/plot

输入三个人们，和他们各自的年龄，按下submit，然后你会看到恭喜信息和一个链接，进入链接，可以看到生成的图表。