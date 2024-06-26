# tcp-client-serv


# Client-Server Socket Library (TCP)

Library written in Python 3.7, for easy client-server communication through sockets (TCP)

### Prerequisites

```server.py``` & ```client.py``` must be in the same folder of your .py file.

To import all classes from server.py:

```python
from server import *
```

To import all classes from client.py:

```python
from client import *
```

## Getting Started

### server.py

There are 3 kinds (classes) of server included in library (```server.py```):

```
Server
IterativeServer
MultithreadedServer
```

To create a server object (i.e. simple ```Server```):

```python
server = Server(port, protocol)
```
or
```python
server = Server(protocol=MyServerProtocol()) # port is by default 1234
```

Same logic for the other two classes (```IterativeServer```, ```MultithreadedServer```).

```listen()```python function establishes connection with client:

```python
server.listen() # server is ready to receive request
```

### client.py

To create an object of Client class:

```python
client = Client(host, port, protocol)
```
or
```python
my_port = 2345
client = Client(port=my_port, protocol=MyClientProtocol()) # host is by default localhost
```
or
```python
client = Client(protocol=MyClientProtocol()) # host is by default localhost, 
                                             # port is by default 1234
```                         

```open()``` function establishes connection with Server (server must probably be listening):

```python
client.open() # client tries to connect with server
              # when connected, sends the request
              # receives reply
              # and closes connection
```

#### Server

```Server``` will wait for a request, will send response (if no errors will occure) and will close the connection. ```Server``` will be used for one and only request.

##### Usage Example 

```python
server = Server(protocol=a_server_protocol())  # create server object (must create a protocol to pass)

server.listen() # server will listen for one request only
```

#### IterativeServer

```IterativeServer``` will serve one client at a time, until it is stopped (CTRL + C).

##### Usage Example

```python
server = IterativeServer(protocol=a_server_protocol())  # create server object (must create a protocol to pass)

server.listen() # server will listen forever, and serve one client each time (not 2 or more parallel)
```
#### MultithreadedServer

```Multithreaded``` server creates a thread, for each request arrives. The difference here (comparing to ```IterativeServer```) is that our server can serve multiple requests in parallel. Server is active until it is stopped (CTRL + C).

##### Usage Example

```python
server = MultithreadedServer(protocol=a_server_protocol())  # create server object (must create a protocol to pass)

server.listen() # server will listen forever, and serve multiple client requests in parallel
```

### Instructions

#### Server

1. Create a class (i.e. ```MyServerProtocol```) & implement ```ServerProtocol``` abstract class.

2. Implement ```process_request()``` function (PyCharm screenshot):

![alt text](https://i.imgur.com/hHnGStx.png)

3. Server knows what to do on request through ```process_request()``` function. ```input_msg``` is client's request.

```python
class MyServerProtocol(ServerProtocol):

  def process_request(self, input_msg):

          output = 'Hello Client!' # prints hello to client, when a request arrives

          return output
```   

4. After finishing 
your protocol, it is time to integrate it in our server:

```python
if __name__ == '__main__':  # server_main.py

    server_protocol = MyServerProtocol() # create protocol we written
    
    server = MultiThreadedServer(protocol=server_protocol) # pass it to server (i.e. MultithreadedServer)
    
    server.listen() # server now listening for requests from clients
    
```   
#### Client

1. Create a class (i.e. ```MyClientProtocol```) & implement ```ClientProtocol``` abstract class.

2. Implement prepare_request & process_reply function (PyCharm screenshot):

![alt text](https://i.imgur.com/ScqG1oR.png)

3. Client knows what kind of request to send through ```prepare_request()``` function, ```process_reply()``` function is for printing server's message. ```input_msg``` (```process_reply()``` parameter) is server's reply.

```python
class MyClientProtocol(ClientProtocol):

    def prepare_request(self):

        output = 'Hello Server!'

        return output

    def process_reply(self, input_msg):
        print('Response: ', input_msg)

```   

4. After finishing writing your protocol, it is time to integrate it in our client object:

```python
if __name__ == '__main__':  # client_main.py

    client_protocol = MyClientProtocol() # create protocol we written
    
    client = Client(protocol=client_protocol) # pass it to client
    
    client.open() # client will now try to establish connection with server. 
                  # If it does, client will send the request to server
                  # When a reply comes back, connection closes
```   

## Credits

Legacy code: [Professor K.G. Margaritis](https://sites.google.com/site/kgmargaritis/)
