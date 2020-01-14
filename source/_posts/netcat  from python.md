---
title: netcat  from python
date: 2019-01-17 17:35:00
categories: 信息安全
tags:
  - netcat
  - hack
  - python
---
# netcat  from python
>  在网络工具中有“瑞士军刀”美誉的NetCat， 在我们用了N年了至今仍是爱不释手。
学习信息安全的应该很熟悉，用来反弹shell。
## Model [socket]
> 网络上的两个程序通过一个双向的通信连接实现数据的交换，这个连接的一端称为一个socket。
 我们需要一个服务端和客户端之间进行通信连接
 支持TCP/UDP协议，在未被关闭TCP的情况下还是使用TCP协议（三次握手还是比较稳的）。
## CODE
>### server.py
```python
Tcp_server = SocketServer.ThreadingTCPServer((str(socket_ip),int(socket_port)),Tcp_socket)
Tcp_server.serve_forever()
```
#### 实现多线程TCP，这样可以多个客户端进行连接。
```python
while True:
            try:
                get_bytes = conn.recv(1024)
                get_info = str(get_bytes)
                if get_info == "q":
                    break
                else:
                    try:
                        command = subprocess.check_output(get_bytes,stderr=subprocess.STDOUT,shell=True,universal_newlines=True)
                        command_info = conn.send(bytes(command))
                    except:
                        return command
                    # try:
                    #     command = os.popen(get_bytes).read()
                    #     command_info = conn.send(bytes(command))
                    # except Exception,e:
                    #     command = os.popen(get_bytes).read()
                    #     command_info = conn.send(bytes(command))                     
            except Exception,e:
                print e
```
 摒弃os.popen，当出现错误命令时导致服务卡死。
>### client.py
将请求发送给服务端，然后接收返回值的一个过程。
```python
while True:
        try:
            sed_data = raw_input(">>>")
            if sed_data == "q":
                conn.send(bytes(sed_data))
                break
            else:
                try:
                    conn.send(bytes(sed_data))
                    get_bytes = conn.recv(1024)
                    get_info = str(get_bytes)
                    print get_info
                except Exception,e:
                    print "Command Not Found !"
                    create_socket(socket_ip,socket_port)
                    # sed_data = raw_input(">>>")
                    # conn.send(bytes(sed_data))
                    # get_bytes = conn.recv(1024)
                    # get_info = str(get_bytes)
                    # print get_info
        except Exception,e:
            print e
```
利用报错处理来重新调用函数来解决输入错误命令卡死的现象。
当输入错误命令时，多输入两次命令让程序进行重新调用函数。