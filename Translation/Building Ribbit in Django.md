###0 设置
除了需要安装Django，我们还需要安装South来控制数据库迁移。使用如下命令：

	pip install Django South

###1 创建项目和Ribbit App
创建一个新的Django项目和app，进入到你选定的目录，使用如下命令：
	
	django-admin.py startproject ribbit
	cd ribbit
	django-admin.py startapp ribbit_app

初始化Git仓库，创建一个.gitignore文件在Ribbit项目中，为此在Git Bash下运行以下命令：

	git init
	echo "*.pyc" >> .gitignore
	git add .
	git commit -m 'Initial Commit'

修改ribbit/settings.py来配置我们的项目。首先定义一些常量。在文件顶部添加以下程序：

	import os
 
	PROJECT_PATH = os.path.dirname(os.path.abspath(__file__))  
	LOGIN_URL = '/' 

PROJECT PATH 会存储 settings.py 存储的地址。这使我们在以后可以使用相对路径。LOGIN_URL指定了网站的根路径用来登录。

配置数据库。选择sqlit3，为此修改DATABASES为以下值：

	DATABASES = {
	    'default': {
	        'ENGINE': 'django.db.backends.sqlite3',  # Add 'postgresql_psycopg2', 'mysql', 'sqlite3' or 'oracle'.
	        'NAME': os.path.join(PROJECT_PATH, 'database.db'),                      # Or path to database file if using sqlite3.
	        'USER': '',                      # Not used with sqlite3.
	        'PASSWORD': '',                  # Not used with sqlite3.
	        'HOST': '',                      # Set to empty string for localhost. Not used with sqlite3.
	        'PORT': '',                      # Set to empty string for default. Not used with sqlite3.
	    }
	}

Django在STATIC_ROOT提到的目录中查找静态文件，文件路由的请求是在STATIC URL中指定的。配置它们如下：

	# Absolute path to the directory static files should be collected to.
	# Don't put anything in this directory yourself; store your static files
	# in apps' "static/" subdirectories and in STATICFILES_DIRS.
	# Example: "/home/media/media.lawrence.com/static/"
	STATIC_ROOT = os.path.join(PROJECT_PATH, 'static')
 
	# URL prefix for static files.
	# Example: "http://media.lawrence.com/static/"
	STATIC_URL = '/static/'
注意 os.path.join()的使用。这个函数使我们可以使用之前定义的PROJECT_PATH来相对的指定路径。

接下来我们需要定义Django需要查找的模版文件。编辑TEMPLATE_DIRS为指定的路径：

	TEMPLATE_DIRS = (
	    # Put strings here, like "/home/html/django_templates" or "C:/www/django/templates".
	    # Always use forward slashes, even on Windows.
	    # Don't forget to use absolute paths, not relative paths.
	    os.path.join(PROJECT_PATH, 'templates')
	)

最后，添加South和ribbit_app到INSTALLED_APPS。

	INSTALLED_APPS = (
	    'django.contrib.auth',
	    'django.contrib.contenttypes',
	    'django.contrib.sessions',
	    'django.contrib.sites',
	    'django.contrib.messages',
	    'django.contrib.staticfiles',
	    'south',
	    'ribbit_app',
	    # Uncomment the next line to enable the admin:
	    # 'django.contrib.admin',
	    # Uncomment the next line to enable admin documentation:
	    # 'django.contrib.admindocs',
	)

现在我们创建在设置中定义的目录，以及创建数据库：
	
	mkdir ribbit/static ribbit/templates ribbit_app/static
	python manage.py syncdb
	python manage.py schemamigration ribbit_app --initial
	python manage.py migrate ribbit_app

命令syncdb会创建 required tables和超级用户。因为我们使用了South作为迁移，我们使用语法schemamigration <app_name> --initial 进行初始迁移，并应用python manage.py migrate ribbit_app。

让我们启动开发服务器，确保一切运行正常：

	python manage.py runserver

可以访问：http://localhost:8000/

此外这个项目树看起来如下：

	ribbit
	|-- manage.py
	|-- ribbit
	|   |-- database.db
	|   |-- __init__.py
	|   |-- __init__.pyc
	|   |-- settings.py
	|   |-- settings.pyc
	|   |-- static
	|   |-- templates
	|   |-- urls.py
	|   |-- urls.pyc
	|   |-- wsgi.py
	|   `-- wsgi.pyc
	`-- ribbit_app
	    |-- __init__.py
	    |-- __init__.pyc
	    |-- migrations
	    |   |-- 0001_initial.py
	    |   |-- 0001_initial.pyc
	    |   |-- __init__.py
	    |   `-- __init__.pyc
	    |-- models.py
	    |-- models.pyc
	    |-- static
	    |-- tests.py
	    `-- views.py

我们可以使用以下命令保存改变：
	
	git add .
	git commit -m 'Created app and configured settings'

###2 基本模版和静态文件
下载源码：[https://github.com/NETTUTS/django-ribbit](https://github.com/NETTUTS/django-ribbit)

拷贝静态文件夹，然后对style.less做一些修改，给flash添加一些样式：

	.flash {
	    padding: 10px;
	    margin: 20px 0;
	    &.error {
	        background: #ffefef;
	        color: #4c1717;
	        border: 1px solid #4c1717;
	    }
	    &.warning {
	        background: #ffe4c1;
	        color: #79420d;
	        border: 1px solid #79420d;
	    }
	    &.notice {
	        background: #efffd7;
	        color: #8ba015;
	        border: 1px solid #8ba015;
	    }
	}

注意更新输入元素的宽度以及添加相同的错误类。注意下面的代码包括需要添加或更新的样式。剩下的代码保持不变。

	input {
	    width: 179px;
	    &.error {
	        background: #ffefef;
	        color: #4c1717;
	        border: 1px solid #4c1717;
	    }
	}

我们也需要增加高度#content.wrapper.panel.right：

	height: 433px;

最后，给页脚图片添加一个右边距：

	footer {
	    div.wrapper {
	        img {
	            margin-right: 5px;
	        }
	    }
	}

在继续之前我们提交改变：
	
	git add .
	git commit -m 'Added static files'

接下来创建一个基础模版，其它模版都继承自它。Django使用自己的模版引擎，在ribbit/templates/base.html中定义模版：

	<!DOCTYPE html>
	<html>
	<head>
	    <link rel="stylesheet/less" href="{{ STATIC_URL }}style.less">
	    <script src="{{ STATIC_URL }}less.js"></script>
	</head>

	<body>
	    <header>
	        <div class="wrapper">
	            <img src="{{ STATIC_URL }}gfx/logo.png">
	            <span>Twitter Clone</span>
	            {% block login %}
	            <a href="/">Home</a>
	            <a href="/users/">Public Profiles</a>
	            <a href="/users/{{ username }}">My Profile</a>
	            <a href="/ribbits">Public Ribbits</a>
	            <form action="/logout">
	                <input type="submit" id="btnLogOut" value="Log Out">
	            </form>
	            {% endblock %}
	        </div>
	    </header>
	    <div id="content">
	        <div class="wrapper">
	            {% block flash %}
	            {% if auth_form.non_field_errors or user_form.non_field_errors or ribbit_form.errors %}
	            <div class="flash error">
	                {{ auth_form.non_field_errors }}
	                {{ user_form.non_field_errors }}
	                {{ ribbit_form.content.errors }}
	            </div>
	            {% endif %}
	            {% if notice %}
	            <div class="flash notice">
	                {{ notice }}
	            </div>
	            {% endif %}
	            {% endblock %}
 
	            {% block content %}
 
	            {% endblock %}
	        </div>
	    </div>
	    <footer>
	        <div class="wrapper">
	            Ribbit - A Twitter Clone Tutorial
	            <a href="http://net.tutsplus.com">
	                <img src="{{ STATIC_URL }}gfx/logo-nettuts.png">
	            </a>
	            <a href="http://www.djangoproject.com/">
	                <img src="https://www.djangoproject.com/m/img/badges/djangomade124x25.gif" border="0" alt="Made with Django." title="Made with Django." />
	            </a>
	        </div>
	    </footer>
	</body>
	</html>

在上面的标记中，{{ STATIC_URL }}输出在settings.py中定义的静态url。另一个特点是块的使用。所有块的内容是子模版从基础模版继承，除非明确定义，否则块不会被覆盖。这给我们一些灵活的替换。我们同样使用if条件的块来检查flash的变量是否为空，然后显示适当的消息。

现在继续提交：

	git add .
	git commit -m 'Created base template'

###创建模型
Django的一个优点是它可以包括相当多的模型和表格，然后可以覆盖不同的目的。对于我们的应用，我们使用User模型，通过UserProfile模型添加一些属性给它。此外，为了管理ribbits，我们会创建一个Ribbit模型。Django提供的User模型提供了存储用户名、密码、首尾名字和邮件地址。可以看一下[API](https://docs.djangoproject.com/en/dev/ref/contrib/auth/#django.contrib.auth.models.User)了解默认提供的功能。将下面的代码添加给ribbit_app/models.py中的模型：

	from django.db import models
	from django.contrib.auth.models import User
	import hashlib
	class Ribbit(models.Model):
	    content = models.CharField(max_length=140)
	    user = models.ForeignKey(User)
	    creation_date = models.DateTimeField(auto_now=True, blank=True)
	class UserProfile(models.Model):
	    user = models.OneToOneField(User)
	    follows = models.ManyToManyField('self', related_name='followed_by', symmetrical=False)
 
	    def gravatar_url(self):
	        return "http://www.gravatar.com/avatar/%s?s=50" % hashlib.md5(self.user.email).hexdigest()
	User.profile = property(lambda u: UserProfile.objects.get_or_create(user=u)[0])

现在分析下Ribbit模型。CharField属性最大有140字符长度来存储内容，ForeignKey提供给User模型，以及DateTimeField，当保存这个模型实例时会自动填充时间。

关于UserProfile模型，我们有一个OneToOne字段，定义一个一对一的关系给User模型，以及一个ManyToMany，用来实现follows/followed_by关系。参数related_name使我们使用我们选择的名字来使用向后关系。我们同样设置symmetrical为False，来保证如果User A Follows B 那么 User B不能自动 Follow A。我们定义一个函数根据用户的地址来获取链接到头像图片，以及一个属性来得到或创建当我们使用语法 <user_object>.profile时。这使我们可以很容易的获取UserProfile属性。下面例子说明如何使用ORM来得到一个用户的follows和它的followed by：

	superUser = User.object.get(id=1)
	superUser.profile.follows.all() # Will return an iterator of UserProfile instances of all users that superUser follows
	superUse.profile.followed_by.all() # Will return an iterator of UserProfile instances of all users that follow superUser

现在我们的模型已经定义了，让我们生成迁移并应用它们：

	python manage.py schemamigration ribbit_app --auto
	python manage.py migrate ribbit_app

在继续之前提交改变：

	git add .
	git commit -m 'Created Models'

###创建表单
Django允许我们创建表单来很容易验证用户的违规行为。我们给Ribbit模型创建一个自定义表单，创建一个表单继承自UserCreationForm用来管理登记。为了管理身份验证，我们会扩展由Django默认提供的AuthenticationForm。让我们创建一个新的文件ribbit_app/forms.py，添加以下代码：

	from django.contrib.auth.forms import AuthenticationForm, UserCreationForm
	from django.contrib.auth.models import User
	from django import forms
	from django.utils.html import strip_tags
	from ribbit_app.models import Ribbit

创建注册表单。我们命名为UserCreateForm，然后程序如下：

	class UserCreateForm(UserCreationForm):
	    email = forms.EmailField(required=True, widget=forms.widgets.TextInput(attrs={'placeholder': 'Email'}))
	    first_name = forms.CharField(required=True, widget=forms.widgets.TextInput(attrs={'placeholder': 'First Name'}))
	    last_name = forms.CharField(required=True, widget=forms.widgets.TextInput(attrs={'placeholder': 'Last Name'}))
	    username = forms.CharField(widget=forms.widgets.TextInput(attrs={'placeholder': 'Username'}))
	    password1 = forms.CharField(widget=forms.widgets.PasswordInput(attrs={'placeholder': 'Password'}))
	    password2 = forms.CharField(widget=forms.widgets.PasswordInput(attrs={'placeholder': 'Password Confirmation'}))
 
	    def is_valid(self):
	        form = super(UserCreateForm, self).is_valid()
	        for f, error in self.errors.iteritems():
	            if f != '__all_':
	                self.fields[f].widget.attrs.update({'class': 'error', 'value': strip_tags(error)})
	        return form
 
	    class Meta:
	        fields = ['email', 'username', 'first_name', 'last_name', 'password1',
                  'password2']
	        model = User
上面的代码中，我们强制的设置一些字段作为强制命令通过传入required=True。此外添加了不同的占位符属性给不同的表单使用的不同小部件。一个error类也添加进来包含有错误提示，通过函数is_valid()来完成。最后在Meta类中，我们指定了表单字段显示的顺序，设置模型的形式应是验证的。

接下来我们给身份验证写一个表单：
	
	class AuthenticateForm(AuthenticationForm):
	    username = forms.CharField(widget=forms.widgets.TextInput(attrs={'placeholder': 'Username'}))
	    password = forms.CharField(widget=forms.widgets.PasswordInput(attrs={'placeholder': 'Password'}))
 
	    def is_valid(self):
	        form = super(AuthenticateForm, self).is_valid()
	        for f, error in self.errors.iteritems():
	            if f != '__all__':
	                self.fields[f].widget.attrs.update({'class': 'error', 'value': strip_tags(error)})
	        return form

AuthenticateForm添加了一些占位符和错误类。

最后，完成表单，接受一个新的Ribbit。

	class RibbitForm(forms.ModelForm):
	    content = forms.CharField(required=True, widget=forms.widgets.Textarea(attrs={'class': 'ribbitText'}))
 
	    def is_valid(self):
	        form = super(RibbitForm, self).is_valid()
	        for f in self.errors.iterkeys():
	            if f != '__all__':
	                self.fields[f].widget.attrs.update({'class': 'error ribbitText'})
	        return form
 
	    class Meta:
	        model = Ribbit
	        exclude = ('user',)
在Meta类中，排除选项防止用户字符段被呈现。我们不需要它，因为Ribbit的用户通过sessions来决定。

让我们提交改变：

	git add .
	git commit -m 'Created Forms'

###完成注册和登录
Django提供了很大的灵活性，让我们开始在ribbit/urls.py中定义一些routes：

	urlpatterns = patterns('',
	    # Examples:
	    url(r'^$', 'ribbit_app.views.index'), # root
	    url(r'^login$', 'ribbit_app.views.login_view'), # login
	    url(r'^logout$', 'ribbit_app.views.logout_view'), # logout
	    url(r'^signup$', 'ribbit_app.views.signup'), # signup
	)
接下来，我们使用刚才创建好的models和forms，然后给每一个route写入对应的views。在ribbit_app/views.py中添加imports：

	from django.shortcuts import render, redirect
	from django.contrib.auth import login, authenticate, logout
	from django.contrib.auth.models import User
	from ribbit_app.forms import AuthenticateForm, UserCreateForm, RibbitForm
	from ribbit_app.models import Ribbit

然后定义index view：

	def index(request, auth_form=None, user_form=None):
    	# User is logged in
	    if request.user.is_authenticated():
	        ribbit_form = RibbitForm()
	        user = request.user
	        ribbits_self = Ribbit.objects.filter(user=user.id)
	        ribbits_buddies = Ribbit.objects.filter(user__userprofile__in=user.profile.follows.all)
	        ribbits = ribbits_self | ribbits_buddies
 
	        return render(request,
	                      'buddies.html',
	                      {'ribbit_form': ribbit_form, 'user': user,
	                       'ribbits': ribbits,
	                       'next_url': '/', })
	    else:
	        # User is not logged in
	        auth_form = auth_form or AuthenticateForm()
	        user_form = user_form or UserCreateForm()
 
	        return render(request,
	                      'home.html',
	                      {'auth_form': auth_form, 'user_form': user_form, })

在index view，我们首先检查用户是否登录和渲染模版。ribbits_self和ribbits_buddies通过 | 进行合并。同时，我们检查一个表单实例是否传递给方法，如果没有，我们就创建一个。这允许我们将表单实例传递给恰当的模版。

让我们继续编辑"home.html"模版，这会用来显示index页面给匿名用户。在ribbit/templates/home.html中，添加以下代码：

	{% extends "base.html" %}
 
	{% block login %}
	    <form action="/login" method="post">{% csrf_token %}
	        {% for field in auth_form %}
	        {{ field }}
	        {% endfor %}
	        <input type="submit" id="btnLogIn" value="Log In">
	    </form>
	{% endblock %}
 
	{% block content %}
	{% if auth_form.non_field_errors or user_form.non_field_errors %}
	<div class="flash error">
	    {{ auth_form.non_field_errors }}
	    {{ user_form.non_field_errors }}
	</div>
	{% endif %}
	<img src="{{ STATIC_URL}}gfx/frog.jpg">
	<div class="panel right">
	    <h1>New to Ribbit?</h1>
	    <p>
	        <form action="/signup" method="post">{% csrf_token %}
	            {% for field in user_form %}
	            {{ field }}
	            {% endfor %}
	            <input type="submit" value="Create Account">
	        </form>
	    </p>
	</div>
	{% endblock %}

在这个模版中，我们插入了之前定义的base模版，通过在逻辑块中重写，渲染验证和注册表单。为了进行CSRF包含。你需要在每一个模版中使用的表单添加一个csrf_token。

让我们继续buddies.html模版，这会展示Buddies的Ribbit页面。编辑ribbit/templates/buddies.html，然后添加以下代码：

	{% extends "base.html" %}
	{% block login %}
	    {% with user.username as username %}
	        {{ block.super }}
	    {% endwith %}
	{% endblock %}
 
	{% block content %}
	    <div class="panel right">
	        <h1>Create a Ribbit</h1>
	        <p>
	            <form action="/submit" method="post">
	            {% for field in ribbit_form %}{% csrf_token %}
	            {{ field }}
	            {% endfor %}
	            <input type="hidden" value="{{ next_url }}" name="next_url">
	            <input type="submit" value="Ribbit!">
	            </form>
	        </p>
	    </div>
	    <div class="panel left">
	        <h1>Buddies' Ribbits</h1>
	        {% for ribbit in ribbits %}
	        <div class="ribbitWrapper">
	            <a href="/users/{{ ribbit.user.username }}">
	                <img class="avatar" src="{{ ribbit.user.profile.gravatar_url }}">
	                <span class="name">{{ ribbit.user.first_name }}</span>
	            </a>
	            @{{ ribbit.user.username }}
	            <p>
	                {{ ribbit.content }}
	            </p>
	        </div>
	        {% endfor %}
	    </div>
	{% endblock %}

在这个模版中，我们提供了父模版base.html。导航链接可以使用户的资料正确的显示。我们还使用了一个RibbitForm的实例来接收新的Ribbits，然后结束循环，展现当前的ribbits。

当完成模版后，继续写log in/out的代码。添加下列代码在ribbit_app/views.py中：

	def login_view(request):
	    if request.method == 'POST':
	        form = AuthenticateForm(data=request.POST)
	        if form.is_valid():
	            login(request, form.get_user())
	            # Success
	            return redirect('/')
	        else:
	            # Failure
	            return index(request, auth_form=form)
	    return redirect('/')
	def logout_view(request):
	    logout(request)
	    return redirect('/')
进行login的views请求HTTP POST进行login。它使表单有效，如果成功，使用login()方法登录，返回到根地址，我们传递auth_form实例到index函数，然后列出错误，如果请求不是POST，然后重新指向根地址。

logout相对简单一些。利用Django的logout函数，它会删除session和logs，重新指向根路径。

我们需要写view给sign up和register a user。在文件ribbit_app/views.py中添加以下代码：

	def signup(request):
	    user_form = UserCreateForm(data=request.POST)
	    if request.method == 'POST':
	        if user_form.is_valid():
	            username = user_form.clean_username()
	            password = user_form.clean_password2()
	            user_form.save()
	            user = authenticate(username=username, password=password)
	            login(request, user)
	            return redirect('/')
	        else:
	            return index(request, user_form=user_form)
	    return redirect('/')

和login view一样，signup view请求POST，如果检查失败重新指向根地址。如果Sign Up表单有效，用户保存到数据库，验证，登录然后指向主页。否则，我们调用index函数，传递用户提交的user_form实例，并列出错误。

通过启动服务器进行检测我们创建的视图：

	python manage.py runserver

提交更改：

	git add .
	git commit -m 'Implemented User Login and Sign Up'

###接收新的Ribbits，列出公开的Ribbits

在我们之前创建的buddies.html中，ribbit_form提交给/submit。我们编辑ribbit/urls.py，然后添加route给表单，这个页面列出所有公开的ribbits：

	urlpatterns = patterns('',
	    # Examples:
	    url(r'^$', 'ribbit_app.views.index'), # root
	    url(r'^login$', 'ribbit_app.views.login_view'), # login
	    url(r'^logout$', 'ribbit_app.views.logout_view'), # logout
	    url(r'^ribbits$', 'ribbit_app.views.public'), # public ribbits
	    url(r'^submit$', 'ribbit_app.views.submit'), # submit new ribbit
	)

写一个view来验证和存储ribbits的提交。打开ribbit_app/views.py，然后添加以下代码：

	from django.contrib.auth.decorators import login_required
 
	@login_required
	def submit(request):
	    if request.method == "POST":
	        ribbit_form = RibbitForm(data=request.POST)
	        next_url = request.POST.get("next_url", "/")
	        if ribbit_form.is_valid():
	            ribbit = ribbit_form.save(commit=False)
	            ribbit.user = request.user
	            ribbit.save()
	            return redirect(next_url)
	        else:
	            return public(request, ribbit_form)
	    return redirect('/')

这个view使用@login required，会用于用户身份的验证。否则用户重新指向在设置中设定的LOGIN URL路径。如果表单设定成功，我们手动设定用户在session中的一个包含，然后保存记录。在数据库提交后，用户指向在next_url中指定的路径，这是一个隐藏的表单,我们为此在这个模版手动输入。next url的值在views中传承下去然后渲染Ribbit Form。

然后我们写一个view列出10个公开Ribbits。在ribbit_app/views.py中添加：

	@login_required
	def public(request, ribbit_form=None):
	    ribbit_form = ribbit_form or RibbitForm()
	    ribbits = Ribbit.objects.reverse()[:10]
	    return render(request,
	                  'public.html',
	                  {'ribbit_form': ribbit_form, 'next_url': '/ribbits',
	                   'ribbits': ribbits, 'username': request.user.username})  

在这个公开view中，我们查询数据库中的10个ribbits，这个表单通过模版渲染，创建ribbit/templates/public.html，代码如下：

	{% extends "base.html" %}
 
	{% block content %}
	    <div class="panel right">
	        <h1>Create a Ribbit</h1>
	        <p>
	            <form action="/submit" method="post">
	            {% for field in ribbit_form %}{% csrf_token %}
	            {{ field }}
	            {% endfor %}
	            <input type="hidden" value="{{ next_url }}" name="next_url">
	            <input type="submit" value="Ribbit!">
	            </form>
	        </p>
	    </div>
	    <div class="panel left">
	        <h1>Public Ribbits</h1>
	        {% for ribbit in ribbits %}
	        <div class="ribbitWrapper">
	            <img class="avatar" src="{{ ribbit.user.profile.gravatar_url }}">
	            <span class="name">{{ ribbit.user.first_name }}</span>@{{ ribbit.user.username }}
	            <span class="time">{{ ribbit.creation_date|timesince }}</span>
	            <p>{{ ribbit.content }}</p>
	        </div>
	        {% endfor %}
	    </div>
	{% endblock %}

在循环中，模版使用timesince模版标签来自动决定时间差，然后以类似Twitter的方式显示。

运行，然后进行测试：http://localhost:8000/ribbits

提交改变：

	git add .
	git commit -m 'Implemented Ribbit Submission and Public Ribbits Views'

###用户资料和Following用户
写一个routes来实现这个功能，在ribbit/urls.py中更新代码：

	urlpatterns = patterns('',
	    # Examples:
	    url(r'^$', 'ribbit_app.views.index'), # root
	    url(r'^login$', 'ribbit_app.views.login_view'), # login
	    url(r'^logout$', 'ribbit_app.views.logout_view'), # logout
	    url(r'^ribbits$', 'ribbit_app.views.public'), # public ribbits
	    url(r'^submit$', 'ribbit_app.views.submit'), # submit new ribbit
	    url(r'^users/$', 'ribbit_app.views.users'),
	    url(r'^users/(?P<username>\w{0,30})/$', 'ribbit_app.views.users'),
	    url(r'^follow$', 'ribbit_app.views.follow'),
	)
<?P<username> 通过GET来获取用户名，然后\w{0,30}声明用户名最大长度为30个字符。

接下来写一个view来显示User Profiles。在ribbit_app/views.py中添加代码：

	from django.db.models import Count
	from django.http import Http404
	def get_latest(user):
	    try:
	        return user.ribbit_set.order_by('-id')[0]
	    except IndexError:
	        return ""
	@login_required
	def users(request, username="", ribbit_form=None):
	    if username:
	        # Show a profile
	        try:
	            user = User.objects.get(username=username)
	        except User.DoesNotExist:
	            raise Http404
	        ribbits = Ribbit.objects.filter(user=user.id)
	        if username == request.user.username or request.user.profile.follows.filter(user__username=username):
	            # Self Profile or buddies' profile
	            return render(request, 'user.html', {'user': user, 'ribbits': ribbits, })
	        return render(request, 'user.html', {'user': user, 'ribbits': ribbits, 'follow': True, })
	    users = User.objects.all().annotate(ribbit_count=Count('ribbit'))
	    ribbits = map(get_latest, users)
	    obj = zip(users, ribbits)
	    ribbit_form = ribbit_form or RibbitForm()
	    return render(request,
	                  'profiles.html',
	                  {'obj': obj, 'next_url': '/users/',
	                   'ribbit_form': ribbit_form,
	                   'username': request.user.username, })

我们开始保证只有用户登录才能看到的资料。在ribbit/urls.py中定义的地址中，我们写了一个来获取用户名。这种获取模式自动的调用用户视图请求对象。对于这个视图，我们有两个选项：

1. 用户名传递给url来渲染一个特定的用户资料
2. 没有传递用户名，表示用户想要看到所有资料

我们检查用户是否通过并且不为空。然后，我们试着获取User对象到对应的用户名。注意上面的代码使用了 try catch。我们加注Http404异常来指向默认的404模版。接下来，我们需要检查资料请求是来自用户还是它的伙伴。那样，我们不需要呈现链接，因为关系已经确定。否则，我们传递follow参数在view视图打印Follow链接。

到第二部分，如果在url中没有给出用户名，我们获取所有用户，然后使用annotate()函数添加一个ribbit count属性给所有对象，这存贮了Ribbits的数量。这允许我们按照<user_object>.ribbit_count使用一些东西来获取the Ribbit Count of the user。

我们使用Python的map()函数，调用get latest()对所有的用户请求元素。这个get_latest()函数定义在代码中的，使用了一个反向关系链在User<–>Ribbit关系。例如user.ribbit_set.all()会返回所有的ribbits。在try catch块中封装的代码来捕获异常，如果用户没有创建ribbits。我们使用zip()函数来链接每个元素的迭代器，我们然后传递zipped对象给模版。我们完成最后的view，用来接收请求来follow一个用户：

	from django.core.exceptions import ObjectDoesNotExist
 
	@login_required
	def follow(request):
	    if request.method == "POST":
	        follow_id = request.POST.get('follow', False)
	        if follow_id:
	            try:
	                user = User.objects.get(id=follow_id)
	                request.user.profile.follows.add(user.profile)
	            except ObjectDoesNotExist:
	                return redirect('/users/')
	    return redirect('/users/')

在上面的view中，我们得到follow参数的值，通过POST传递。我们设定的id存在，添加这个关系，否则出现ObjectDoesNotExist异常，然后切除所有的用户资料。

我们写两个模版来显示用户资料，编辑ribbit/templates/profiles.html：

	{% extends "base.html" %}
 
	{% block content %}
	    <div class="panel right">
	        <h1>Create a Ribbit</h1>
	        <p>
	            <form action="/submit" method="post">
	            {% for field in ribbit_form %}{% csrf_token %}
	            {{ field }}
	            {% endfor %}
	            <input type="hidden" value="{{ next_url }}" name="next_url">
	            <input type="submit" value="Ribbit!">
	            </form>
	        </p>
	    </div>
	    <div class="panel left">
	        <h1>Public Profiles</h1>
	        {% for user, ribbit in obj %}
	        <div class="ribbitWrapper">
	            <a href="/users/{{ user.username }}">
	                <img class="avatar" src="{{ user.profile.gravatar_url }}">
	                <span class="name">{{ user.first_name }}</span>
	            </a>
	            @{{ user.username }}
	            <p>
	                {{ user.ribbit_count}} Ribbits
	                <span class="spacing">{{ user.profile.followed_by.count }} Followers</span>
	                <span class="spacing">{{ user.profile.follows.count }} Following</span>
	            </p>
	            <p>{{ ribbit.content }}</p>
	        </div>
	        {% endfor %}
	    </div>
	{% endblock %}

我们构件遍历一个列表的元祖：for user, ribbit in obj。在循环中我们打印用户信息和状态。注意在{{ user.profile.followed_by.count }}. followed_by中使用的关系，我们对所有的UserProfile Model设定ManyToManyField属性。

最后，我们写一个模版来列出特定用户的资料。完成代码ribbit/templates/user.html：

	{% extends "base.html" %}
	{% block login %}
	    {% with user.username as username %}
	        {{ block.super }}
	    {% endwith %}
	{% endblock %}
 
	{% block content %}
	    <div class="panel left">
	        <h1>{{ user.first_name }}'s Profile</h1>
	        <div class="ribbitWrapper">
	            <a href="/users/{{ user.username }}">
	                <img class="avatar" src="{{ user.profile.gravatar_url }}">
	                <span class="name">{{ user.first_name }}</span>
	            </a>
	                @{{ user.username }}
	            <p>
	                {{ ribbits.count }} Ribbits
	                <span class="spacing">{{ user.profile.follows.all.count }} Following</span>
	                <span class="spacing">{{ user.profile.followed_by.all.count }} Followers</span>
	            </p>
	            {% if follow %}
	            <form action="/follow" method="post">
	                {% csrf_token %}
	                <input type="hidden" name="follow" value="{{ user.id }}">
	                <input type="submit" value="Follow">
	            </form>
	            {% endif %}
	        </div>
	    </div>
 
	    <div class="panel left">
	        <h1>{{ user.first_name }}'s Ribbits</h1>
	        {% for ribbit in ribbits %}
	        <div class="ribbitWrapper">
	            <a href="/users/{{ user.username }}">
	                <img class="avatar" src="{{ user.profile.gravatar_url }}">
	                <span class="name">{{ ribbit.user.first_name }}</span>
	            </a>
	            @{{ ribbit.user.username }}
	            <span class="time">{{ ribbit.creation_date|timesince }}</span>
	            <p>{{ ribbit.content }}</p>
	        </div>
	        {% endfor %}
	    </div>
	{% endblock %}

在buddies.html模版中，我们传递登录的用户名到父模版通过使用 {% with %}结构。接下来，我们显示用户资料，显示follow链接，只有follow 设置为True。之后，列出所有的创建的ribbbits，迭代ribbits变量。

现在已经完成了。

提交改变：

	git add .
	git commit -m 'Implemented User Profiles and Following Relation on frontend'

###在Heroku中部署
使用git分支，使用下面命令：

	git branch -b production

更新.gitignore文件，然后添加数据库:

	*.pyc
	ribbit/database.db

安装一些包，来进行部署：

	pip install psycopg2 dj-database-url gunicorn

创建一个文件包括项目所需要的项目依赖关系，需要在Heroku上部署。

	pip freeze > requirements.txt

接下来修改ribbit/settings.py，数据库的更改如下：
	
	DATABASES = {
	    'default': {
	        'ENGINE': 'django.db.backends.postgresql_psycopg2', # Add 'postgresql_psycopg2', 'mysql', 'sqlite3' or 'oracle'.
	        'NAME': 'ribbit',                      # Or path to database file if using sqlite3.
	        'USER': 'username',                      # Not used with sqlite3.
	        'PASSWORD': 'password',                  # Not used with sqlite3.
	        'HOST': '',                      # Set to empty string for localhost. Not used with sqlite3.
	        'PORT': '',                      # Set to empty string for default. Not used with sqlite3.
	    }
	}
在文件最后附加如下代码，那么Heroku能够配置数据库：

	import dj_database_url
 
	DATABASES['default'] = dj_database_url.config()

创建一个Procfile来启动Herorku服务，将以下代码放入Procfile文件：
	
	web: gunicorn ribbit.wsgi

如果你的应用规模不大，你可以略过使用存储服务通过在ribbit/urls.py中加入如下代码：

	urlpatterns += patterns('django.contrib.staticfiles.views',
	        url(r'^static/(?P<path>.*)$', 'serve'),
	    )

提交改变：

	git commit -a -m 'Configured for Production'

最后创建一个Heroku App，推送到远程代码仓库：

	heroku create
	git push heroku production:master

一旦完成，运行syncdb，应用数据库牵引：

	heroku run python manage.py syncdb
	heroku run python manage.py migrate ribbit_app

最后，打开部署的应用：
	
	heroku open

###恭喜你
你已经创建成功了。

链接：[Building Ribbit in Django](http://net.tutsplus.com/tutorials/python-tutorials/building-ribbit-with-django/)