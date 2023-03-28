# 使用 Python 创建视频聊天/视频流应用程序

> 原文：<https://medium.com/geekculture/creating-video-chat-app-using-python-9da0a9c386ba?source=collection_archive---------2----------------------->

由于疫情，通过互联网保持联系的唯一方式。但是由于广告部的活动如此之大，数据泄露和数据隐私是一个大问题。为了克服数据隐私问题，让我们创建自己的视频聊天应用程序，以便与我们亲爱的人保持联系，并避免被任何广告公司跟踪。之前我们创建了一个文本聊天应用程序，现在让我们向前迈出一步，创建一个视频聊天应用程序。

> 继续之前，请参考我的聊天应用程序:[使用 UDP 创建 Python 聊天应用程序| gur simar Singh | Python 简明英语版](https://python.plainenglish.io/chat-app-using-udp-5b486241748c)

让我们看看代码，

我们将使用 OpenCV 库的 CV2 模块来捕捉视频。

> 了解有关 CV2 的更多信息:[使用 Python 中的 OpenCV 进行图像处理|作者 Gursimar Singh | Jun，2021 |用简明英语讲述 Python](https://python.plainenglish.io/image-processing-using-opencv-in-python-857c8cb21767)

建议创建一个单独的环境来安装所需的库，以便在出现任何错误时不会干扰默认环境

**Server.py**

```
from pyfiglet import Figletos.system("clear")
pyf = Figlet(font='puffy')
a = pyf.renderText("Video Chat App without Multi-Threading")
b = pyf.renderText("Server")
os.system("tput setaf 3")
print(a)import socket, cv2, pickle,struct# Socket Create
server_socket = socket.socket(socket.AF_INET,socket.SOCK_STREAM)
host_name  = socket.gethostname()
host_ip = socket.gethostbyname(host_name)
print('HOST IP:',host_ip)
port = 9999
socket_address = (host_ip,port)# Socket Bind
server_socket.bind(socket_address)# Socket Listen
server_socket.listen(1)
print("Listening at:",socket_address)# Socket Accept
while True:
 client_socket,addr = server_socket.accept()
 print('Connected to:',addr)
 if client_socket:
  vid = cv2.VideoCapture(0)

  while(vid.isOpened()):
   ret,image = vid.read()
   img_serialize = pickle.dumps(image)
   message = struct.pack("Q",len(img_serialize))+img_serialize
   client_socket.sendall(message)

   cv2.imshow('Video from Server',image)
   key = cv2.waitKey(10) 
   if key ==13:
    client_socket.close()
```

**Client.py**

```
from pyfiglet import Figletos.system("clear")
pyf = Figlet(font='puffy')
a = pyf.renderText("Video Chat App without Multi-Threading")
b = pyf.renderText("Client")
os.system("tput setaf 3")
print(a)import socket,cv2, pickle,struct# create socket
client_socket = socket.socket(socket.AF_INET,socket.SOCK_STREAM)#  server ip address here
host_ip = '<IP>' 
port = 9999
client_socket.connect((host_ip,port)) 
data = b""
metadata_size = struct.calcsize("Q")while True:
 while len(data) < metadata_size:
  packet = client_socket.recv(4*1024) 
  if not packet: break
  data += packet
 packed_msg_size = data[:metadata_size]
 data = data[metadata_size:]
 msg_size = struct.unpack("Q",packed_msg_size)[0]

 while len(data) < msg_size:
  data += client_socket.recv(4*1024)
  frame_data = data[:msg_size]
  data  = data[msg_size:]
  frame = pickle.loads(frame_data)
  cv2.imshow("Receiving Video ",frame)
  key = cv2.waitKey(10) 
  if key  == 13:
   breakclient_socket.close()
```

我们运行 server.py，然后我们尝试运行 client.py，图片传输速度惊人，因此创建了一个视频流。

这里我们首先点击图片，然后使用 pickle 模块将其从数组格式转换为字节格式

```
#Converting this image to bytes format 
import pickle
photo_serialize=pickle.dumps(photo)
```

现在我们需要提供缓冲区大小，以便客户端可以接收数据。为此，我们使用 Python 中的 struct 模块。这里“Q”是我们可以在参数中传递的值，因此它提供了 8 个字节。

我们使用 struct 模块中的 pack()函数打包数据，前 8 个字节对应于文件的大小。

我们使用 unpack 函数来解包打包的数据。

```
import struct
size = struct.calcsize("Q")message = struct.pack("Q",len(img_serialize))+img_serializemsg_size = struct.unpack("Q",packed_msg_size)[0]
```

## 相反，我们也可以使用 flatten()和 tostring()函数。

```
def recordVideo():
    time.sleep(5)    
    while True:
        ret, frame = cap.read()
        d = frame.flatten()
        video = d.tostring()
        c.sendall(video)
        time.sleep(0.2)def rcvVideo():
     while True:
          data, addr = s.recvfrom(230400)
          frames = ""
          frames += data
          if len(frames) == (230400):frame = numpy.fromstring (frames,dtype=numpy.uint8)
              frame = frame.reshape (240,320,3)
              cv2.imshow('frame',frame)
              frames=""
              cv2.waitKey(1)
          else:
              frames=""
```

**现在，让我们使用多线程创建一个流畅的视频聊天应用程序。我们将在客户端 A 和客户端 B 中同时启动 4 个线程(2 个线程用于视频流和接收，2 个线程用于音频流和收听)。一个线程将发送视频，另一个线程将接收视频。**

我们将使用 PyAudio 库进行音频传输。现在，安装它可能是棘手的，所以只使用下面的命令安装它，否则你可能会打破你的头，仍然无法找到一个稳定的版本。

```
$ conda install -c anaconda pyaudio
```

为此，我们需要安装 Anaconda 发行版。Anaconda 有一个更新的无 bug 版本的 PyAudio 库。

## 客户 A

```
import os
from pyfiglet import Figletos.system("clear")
pyf = Figlet(font='puffy')
a = pyf.renderText("UDP Chat App with Multi-Threading")
os.system("tput setaf 3")
print(a)import socket, cv2, pickle, struct, threading, time# Socket Create
s = socket.socket(socket.AF_INET,socket.SOCK_STREAM)# Socket Accept
def sender():
 time.sleep(15)
 host_name  = socket.gethostname()
 host_ip = socket.gethostbyname(host_name)
 print('Host IP:',host_ip)
 port = 9999
 socket_address = (host_ip,port)# Socket Bind
 s.bind(socket_address)# Socket Listen
 s.listen(5)
 print("Listening at:",socket_address)
 while True:
  client_socket,addr = s.accept()
  print('Connection to:',addr)
  if client_socket:
   vid = cv2.VideoCapture(0)

   while(vid.isOpened()):
    ret,image = vid.read()
    img_serialize = pickle.dumps(image)
    message = struct.pack("Q",len(img_serialize))+img_serialize
    client_socket.sendall(message)

    cv2.imshow('Video from server', image)
    key = cv2.waitKey(10) 
    if key ==13:
     client_socket.close()#Audio
chunk = 1024
FORMAT = pyaudio.paInt16
CHANNELS = 1
RATE = 44100p = pyaudio.PyAudio()stream = p.open(format = FORMAT,
                channels = CHANNELS,
                rate = RATE,
                input = True,
                frames_per_buffer = chunk)#Audio Socket Initialization
audioSocket = socket.socket()
port1 = 5000
audioSocket.bind((<IP>,port1))
audioSocket.listen(5)
cAudio, addr = audioSocket.accept()def recordAudio():
    time.sleep(5)
    while True:
        data = stream.read(chunk)
        if data:
            cAudio.sendall(data)def rcvAudio():
     while True:
          audioData = audioSocket.recv(size)
          stream.write(audioData)def connect_server():
 host_ip = '<IP>' 
 port = 1234
 s.connect((host_ip,port)) 
 data = b""
 metadata_size = struct.calcsize("Q")
 while True:
  while len(data) < metadata_size:
   packet = s.recv(4*1024) 
   if not packet: break
   data+=packet
  packed_msg_size = data[:metadata_size]
  data = data[metadata_size:]
  msg_size = struct.unpack("Q",packed_msg_size)[0]

  while len(data) < msg_size:
   data += s.recv(4*1024)
  frame_data = data[:msg_size]
  data  = data[msg_size:]
  frame = pickle.loads(frame_data)
  cv2.imshow("Receiving Video",frame)
  key = cv2.waitKey(10) 
  if key  == 13:
   break
 s.close()x1 = threading.Thread(target = sender)
x2 = threading.Thread(target = connect_server)
x3 = threading.Thread(target = recordAudio)
x4 = threading.Thread(target = rcvAudio)# start a thread
x1.start()
x2.start()
x3.start()
x4.start()
```

## 客户 B

```
import os
from pyfiglet import Figletos.system("clear")
pyf = Figlet(font='puffy')
a = pyf.renderText("UDP Chat App with Multi-Threading")
os.system("tput setaf 3")
print(a)import socket, cv2, pickle, struct, time, threading# create socket
s = socket.socket(socket.AF_INET,socket.SOCK_STREAM)def connect_server():
 time.sleep(15)
 host_ip = '192.168.99.1' 
 port = 9999
 s.connect((host_ip,port)) 
 data = b""
 metadata_size = struct.calcsize("Q")
 while True:
  while len(data) < metadata_size:
   packet = s.recv(4*1024) 
   if not packet: break
   data+=packet
  packed_msg_size = data[:metadata_size]
  data = data[metadata_size:]
  msg_size = struct.unpack("Q",packed_msg_size)[0]

  while len(data) < msg_size:
   data += s.recv(4*1024)
   frame_data = data[:msg_size]
   data  = data[msg_size:]
   frame = pickle.loads(frame_data)
   cv2.imshow("Receiving Video", frame)
   key = cv2.waitKey(10) 
   if key  == 13:
    break
 s.close()def sender():
 host_name  = socket.gethostname()
 host_ip = socket.gethostbyname(host_name)
 print('Host IP:',host_ip)
 port = 1234
 socket_address = (host_ip,port)
 # Socket Bind
 s.bind(socket_address)
 # Socket Listen
 s.listen(5)
 print("Listening at:",socket_address)
 while True:
  client_socket,addr = s.accept()
  print('Connected to:',addr)
  if client_socket:
   vid = cv2.VideoCapture(1)

   while(vid.isOpened()):
    ret,image = vid.read()
    img_serialize = pickle.dumps(image)
    message = struct.pack("Q",len(img_serialize))+img_serialize
    client_socket.sendall(message)

    cv2.imshow('Video from server',image)
    key = cv2.waitKey(10) 
    if key ==13:
     client_socket.close()#Audio
chunk = 1024
FORMAT = pyaudio.paInt16
CHANNELS = 1
RATE = 44100p = pyaudio.PyAudio()stream = p.open(format = FORMAT,
                channels = CHANNELS,
                rate = RATE,
                input = True,
                frames_per_buffer = chunk)#Audio Socket Initialization
audioSocket = socket.socket()
port1 = 5000
audioSocket.bind((<IP>,port1))
audioSocket.listen(5)
cAudio, addr = audioSocket.accept()def recordAudio():
    time.sleep(5)
    while True:
        data = stream.read(chunk)
        if data:
            cAudio.sendall(data)def rcvAudio():
     while True:
          audioData = audioSocket.recv(size)
          stream.write(audioData)x1 = threading.Thread(target = connect_server)
x2 = threading.Thread(target = sender)
x3 = threading.Thread(target = recordAudio)
x4 = threading.Thread(target = rcvAudio)x1.start()
x2.start()
x3.start()
x4.start()
```

在 PyAudio 库中，我们需要指定格式、通道、速率、输入和帧/缓冲区。

我们为音频创建了一个单独的套接字，这样它就不会干扰视频流。

现在我们可以在不同的计算机上运行 ClientA.py 和 ClientB.py，玩得很开心。