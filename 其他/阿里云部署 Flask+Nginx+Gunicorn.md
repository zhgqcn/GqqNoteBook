# 阿里云部署：`Flask`+ `Nginx` + `Gunicorn`

> 最近开始学习`FLASK`，虽然有`ubuntu`虚拟机，但是外出实习笔记本不方便携带，手上正好有一台阿里云服务器配有**`Centos环境`**，所以想在阿里云上部署然后方便学习。

**先看看成功后的图：实现利用`公网ip`进行访问**

![Snipaste_2020-12-21_21-33-17](https://tva1.sinaimg.cn/large/005tpOh1ly1glvsxt3ms3j30yn0ag0u4.jpg)

## 项目布局

> 仅用于进行测试，不是全部项目
>
> 项目布局以[Flask官方文档](https://dormousehole.readthedocs.io/en/latest/tutorial/layout.html)推荐的为主

![Snipaste_2020-12-21_21-42-56](https://tvax4.sinaimg.cn/large/005tpOh1ly1glvt7xflszj30hk05ht94.jpg)

## Flask环境安装

推荐使用虚拟环境进行安装

1. 创建项目文件夹并创建虚拟环境

   ```shell
   mkdir flask-tutorial
   cd flask-tutorial
   python3 -m venv venv
   ```

2. 激活虚拟环境

   ```shell
   . venv/bin/activate
   ```

3. 安装Flask

   ```shell
   pip install Flask
   ```

## `gunicorn`安装

1. `gunicorn`安装	

   ```
   pip install gunicorn
   ```

2. 创建一个简单的测试文件`test.py`来看看`gunicorn`是否安装成功

   ```python
   # test.py
   
   from flask import Flask
    
   app = Flask(__name__)
    
   @app.route("/")
   def hello():
       return "<h1>Hello World</h1>"
   ```

   尝试运行：

   ```python
   gunicorn -w 4 -b 127.0.0.1:5000 test:app
   ```

   若没有报错即为成功安装，根据提示`ctrl + c`退出

   - -w 4 ：表示 4 个进程
   - -b 127.0.0.1:5000 ：表示flask使用5000端口
   - `test:app` ：test为程序名，app为flask的实例对象

## `nginx`环境安装

1. 安装

   ```
   sudo yum install nginx
   ```

2. 配置文件

   ```python
   vim /etc/nginx/conf.d/default.conf
   ```

   配置文件如下：

   ```shell
   server {
   	listen	80;
   	server_name 你的服务器的ip地址;
   	location / {
   	proxy_pass http://127.0.0.1:5000;
   	}
   }
   ```

3. 重启

   ```
   systemctl restart nginx.service
   nginx -s reload
   ```

## 配置web访问端口

1. 设置`iptables`

   - 安装

     ```
     yum install iptables-services  
     ```

   - 修改配置文件

     ```
     vim /etc/sysconfig/iptables  
     ```

   - 加入以下配置项

     ```
     -A INPUT -p tcp -m state --state NEW -m tcp --dport 80 -j ACCEPT
     ```

   - 重启服务

     ```
     systemctl restart iptables  
     ```

2. 配置防火墙`firewall`

   - 启动

     ```python
     systemctl start firewalld
     ```

   - 配置

     ```python
     firewall-cmd  --add-port=80/tcp --permanent
     ```

   - 重启

     ```
     systemctl restart firewalld  
     ```

   - 查看是否配置成功

     ```python
     firewall-cmd --list-all
     ```

3. 在阿里云控制台里配置安全组

   ![Snipaste_2020-12-21_22-13-20](https://tvax4.sinaimg.cn/large/005tpOh1ly1glvu3hk3khj31hc0qcn1c.jpg)

   ![Snipaste_2020-12-21_22-11-10](https://tvax4.sinaimg.cn/large/005tpOh1ly1glvu3n9zqdj317z01odfx.jpg)

## 运行`test.py`

```python
gunicorn -w 4 -b 127.0.0.1:5000 test:app
```

通过`http://你的服务器ip地址:80`就可以访问了,能看到 **hello world** 就成功了

🛑但是，到目前为止，我们所运行的只是一个单一的小程序而不是工程项目，而**本文的重点是工程项目的配置**



## 工程项目

1. 创建`flaskr`目录，并创建`__init__.py`

    ```shell
    mkdir flaskr
    cd flaskr
    ```

    ```python
    # __init__.py

    import os 
    from flask import Flask

    def create_app(test_config = None):
        # Create and configure the app
        app = Flask(__name__, instance_relative_config = True)
        app.config.from_mapping(
            SECRET_KEY = 'dev',
            DATABASE = os.path.join(app.instance_path, 'flaskr.sqlite'),
        )

        if test_config is None:
            app.config.from_pyfile('config.py', silent=True)
        else:
            app.config.from_mapping(test_config)

        try:
            os.makedirs(app.instance_path)
        except OSError:
            pass

        @app.route('/')
        def hello():
            return 'Hello World'

        @app.route('/success')
        def success():
            return 'Gqq, you have successed to deploy the environment.'

        from . import db
        db.init_app(app)

        return app
    ```

2. 如果按照上面的经验访问应该是`gunicorn -w 4 -b 127.0.0.1:5000 flaskr.__init__:create_app`，然而这出错了，所以不能对`gunicore`进行简单的参数设置

   ![Snipaste_2020-12-21_22-22-51](https://tvax4.sinaimg.cn/large/005tpOh1ly1glvuddtbk8j30jr05n0t3.jpg)

3. 创建`server.py`用来配置

   ```python
   from flaskr.__init__ import create_app
   
   if __name__ == '__main__':
       create_app = create_app()
       create_app.run()
   else:
       gunicorn_app = create_app()
   ```

4. 执行命令

   ```shell
   gunicorn -w 4 -b 127.0.0.1:5000 server:gunicorn_app
   ```

   此时，便可以成功运行啦。

   建议把上述命令放大`bash.sh`中，之后只需执行`. bash.sh`就可以不用记住那么长的命令

## 一个不错软件

> 为了方便远程连接服务器，在本地下载[`MobaXterm`](https://mobaxterm.mobatek.net/)非常好用便捷，比`Xshell`好用多了

![Snipaste_2020-12-21_22-32-36](https://tvax4.sinaimg.cn/large/005tpOh1ly1glvunjsduyj31hc0saacx.jpg)



> 本文参考：
>
> - [How to get Flask app running with gunicorn](https://stackoverflow.com/questions/51395936/how-to-get-flask-app-running-with-gunicorn)
> - [阿里云部署flask_极简](https://blog.csdn.net/qq_37876050/article/details/105122274?utm_medium=distribute.pc_relevant.none-task-blog-baidujs_title-14&spm=1001.2101.3001.4242)
> - [Flask官方文档](https://dormousehole.readthedocs.io/en/latest/tutorial/layout.html)

> Write by **`Gqq`**