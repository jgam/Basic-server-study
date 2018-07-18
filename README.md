# Basic-server-study
Strive for progress, not perfection

1. "... a better developer must get a better understanding of the underlying software systems you use on a daily basis and thati ncludes programming languages, compilers and interpreters, databases and operating systems, web servers and seb frameworks..."

Web Server is a networking server that sits on a physical server and waits for a client to send a request.
When receiving a request(HTTP), the server generates a response and sends it back to the client.

Now, simple implementation of a Web server?
-> socket!(coonnection to the server)

once called socket, we can use its families including socket, setsockopt, bind, listen and etc to connect to the server.
Important note is the loop that keeps digging for the HTTP request!
while True:
  client_connection, client_address = listen_socket.accept()
  request = client_connection. recv(1024)
  print request
...
client_connection.seendall(http_response)
client_connection.close()

This briefly demonstrates how socket works and implements web server in a precise way!

Some Terms!

http -> HTTP protocol
localhost -> host name
8888 -> port
/hello -> path
URL is the web address!

# Now the question is, how to run frame works without making a change to the server?

2. Python Web Server Gateway Interface also called as "WSGI"
-> allows developers to separate choice of a web framework from choice of a web server.

Now, how do we use WSGI? Let's take a look at the following code:

class WSGIServer(object):
  address_family = socket.AF_INET
  socket_type = socket.SOCK_STREAM
  request_queue_size = 1
  
  def __init__...#it creates a listening socket
    self.listen_socket = listen_socekt = socket.socket(
      self.address_family,
      self.socket_type
      )
      
      #it reuses the same address, binds, and activates with setsockopt, bind and listen function
      #gets server host name and port
      #finally, returns headers set by Web Fremework/web application
      self.headers_set = []
     
  def set_app(self, application):
    self.application = application
  
  def serve_forever(self):
    listen_socket = self.listen_socket
    while True:
      #New client connection
      self.client_connection, client_address = listen_socket.accept()
      #handle one request and close the client connection then loop over to wait for another client connection
      self.handle_one_request()
  
  def handle_one_request(self):
    self.requet_data = request_data = self.client_connection.recv(1024)
    self.parse_request(request_data)
    #constructs environment dictionary using request data
    #call application callable and get back a result that becomes HTTP response body
    result = self.application(env, self.start_response)
    self.finish_response(result)
  
  def parse_request(self, text):
    request_line = text.splitlines()[0]
    request_line = request_line.rstrip('\r\n')
    #breaks down the request line into components
    (self.request_method,
      self.path,
       self.request_version
       ) = request_line.split()
  
  def get_environ(self):
    env = {}
    #setting WSGI variables..
    return env
    
  def start_response(self, status, response_headers, exc_info=None):
    #adds necessary server headers
    server_headers = [('Date', 'Tue, 31 Mar 2015..'),('Server','WSGIServer 0.2')],
    self.headers_set = [status, response_headers + server_headers]
    
  def finish_response(self, result):
    try:
      status, response_headers = self.headers_set
      response = 'HTTP/1.1 {status}\r\n'.format(status=status)
      # this function formats result(response) into the likely format so that we can client_connection.sendall(response)
      
  def make_server(server_address, application):
     server = WSGIServer(server_address)
     server.set_app(application)
     return server

This code shown above demonstrates how to implement and to parse the arguments into the right response.
Now, I create a django framework model and will connect to the program.
Due to the version difference, I had to manually fix some of the changes but they were minimal.
Now when you run the above code taking in parameter as djangoapp:app, it automatically goes into url-> views.py
which outputs Hello world from django!

Now, I learned that WSGI allows me to mix and match the web servers and web frameworks. WSGI provides a minimal interface between
Python Web servers and Python Web Frameworks. It is easy to implement on both the server and the framework side.

A Web framewokr uses the information from that dictionary to decide which view to use based on the specified route, 
request method, where to read the request body from and where to write errors, if any.
