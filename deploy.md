[TOC]

# **首先确保项目在本地能成功运行！！**

## **将本地文件复制到服务器**

```python
# 将本地/home/tarena/下的项目压缩包复制到服务器下的/home/ubuntu/目录下
scp /home/tarena/Letu1.0.tar.xz ubuntu@139.155.80.41:/home/ubuntu/
```

## **解压，权限**

```python
# 将项目解压到/home/ubuntu/目录下
sudo tar -xf Letu1.0.tar.xz -C /home/ubuntu/
# 权限
sudo chown -R ubuntu：ubuntu /home/ubuntu/Letu1.0
```

## **安装pip3,Django**

```python
sudo apt-get install python3-pip
sudo pip3 install Django==1.11.8
```

## **安装PIL图像处理库**

```python
sudo apt-get install libjpeg8 libjpeg62-dev libfreetype6 libfreetype6-dev 
sudo pip install Pillow
```

## **测试安装项目软件包**

```python
# 在项目目录下启动项目，查看错误信息
python3 manage.py runserver
# 根据错误信息安装软件包
```

## **安装uwsgi**

```python
# 安装uwsgi
sudo pip3 install uwsgi
```

## **在项目目录：/home/ubuntu/Letu1.0/Travel/Travel/ 下添加uwsgi.ini文件** 

```python
[uwsgi]
#套接字方式的 IP地址:端口号
socket=127.0.0.1:8881
#Http通信方式的 IP地址:端口号
#http=127.0.0.1:8881
#项目当前工作目录
chdir=/home/ubuntu/Letu1.0/Travel
#程序启动文件,通常在本地是通过运行
wsgi-file=Travel/wsgi.py
#进程个数
process=4
#每个进程的线程个数
threads=1
#服务的pid记录文件
pidfile=uwsgi.pid
#服务的目志文件位置
daemonize=uwsgi.log
```

## **测试uwsgi是否成功**

```python
# 先注释掉uwsgi.ini文件中socket,打开http=0.0.0.0:8000，启动uwsgi，在本地浏览器访问139.155.80.41:8881
uwsgi --ini uwsgi.ini
```

## **启动uwsgi，关闭uwsgi**

```python
# 启动
sudo uwsgi --ini Travel/uwsgi.ini
# 查看uwsgi进程
ps -aux|grep uwsgi
# 关闭
sudo uwsgi --stop uwsgi.pid
sudo killall uwsgi
```

## **安装nginx**

```python
# 安装
sudo apt-get install nginx
```

## **修改配置文件**

```python
sudo vim /etc/nginx/sites-available/default
# 注释掉原来的 location / , 添加新的location / , 添加静态文件配置 location /static
		#location / {
                # First attempt to serve request as file, then
                # as directory, then fall back to displaying a 404.
        #       try_files $uri $uri/ =404;
        #}
        location / {
                uwsgi_pass 127.0.0.1:8881;
                include /etc/nginx/uwsgi_params;
        }
        location /static {
                root /home/ubuntu/Letu1.0/Travel;
        }

```

## **关闭uwsgi和nginx**

```python
sudo killall uwsgi
sudo killall nginx
```

## **重新启动uwsgi和nginx**

```python
sudo uwsgi --ini Travel/uwsgi.ini
sudo /etc/init.d/nginx start
```

# **数据库**

## **数据库文件导出**

```python
# 直接在终端
mysqldump -uroot -p letu > letu.sql
```

## **数据库导入**

```python
# 先创建数据库，use 数据库
mysql>source /home/ubuntu/letu.sql
```

