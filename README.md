java c
159.352 Topic 4 — Exercises 
Transport Layer Security and HTTP
Here   we   will   write   a secure HTTP   server   in   Python.
HTTP server 
Here we   continue   using the   Python http.server module.    Import the   module
import http .server 
Define   a   simple“handler”   class   as   before
class MyHandler(http .server .BaseHTTPRequestHandler): 
def do_GET(self): 
self.send_ response(200) 
self.send_header( ' Content-type ' , ' text/html ' ) 
self.end_headers() 
respbody = f ' 
URI    path :    {self.path} ' self.wfile .write(respbody .encode( ' utf-8 ' ))
Get a multithreaded server object   and   start   serving.    In this   case   we   will   listen   on   port   4443.
webServer = ThreadedHTTPServer(( ' ' ,    4443), MyHandler) 
webServer .serve_forever() 
As   before,   connect   to   the   server   using   a   browser   and/or   curl.
HTTPS server Now   we   will   make   this   an   HTTPS   server   by   wrapping   the   underlying   socket   with   a   TLS   layer.    First   check   that   you   have   OpenSSL   installed   on   your   system.    In   a   terminal   window   type
openssl help 
If you   don’t   have   this   installed,   then   follow   the   instructions   at:   https://www   . openssl   . org/source/gitrepo   . html
Now   we   need   to   generate   a   certificate   and   key.   Make   a   n代 写159.352 Topic 4 — Exercises Transport Layer Security and HTTPPython
代做程序编程语言ew   sub-directory tmp and   issue   the   following   command   from   a   terminal   windowopenssl    req -x509 -nodes -days 365 -newkey rsa:2048 -keyout tmp/key.pem -out tmp/cert.pem 
Verify   that   the   two   files key.pem and cert.pem have    been    created.       Use   these   to    create   a   “context”   in   your   Python   code—make   sure   to   first   import   the ssl module:
context = ssl.SSLContext(ssl.PROTOCOL_TLS_SERVER) 
context.load_cert_chain(certfile= ' tmp/cert.pem ' , keyfile= ' tmp/key .pem ' )
The   socket   used   for the   connections   is   an   attribute   of the webServer object.      Use the   context   to   wrap   this   socket   under   a   secure   layer
webServer .socket = context.wrap_socket(webServer .socket , server_side=True) 
Start   the   server   again.   Now   you   have   an   HTTPS   server   running.   What   happens   when   you   try   to   connect   using?
curl http://localhost:4443 
Now   try:
curl https://localhost:4443 
You   should   find   that   curl   will   not   allow   you   access   to   the   site   because   the   site   certificate   is self-signed.   You   can   override   this   with   the -k switch,   i.e.
curl -k    https://localhost:4443 
Try   connecting   using   various   browsers   and   compare   their   behaviours.





         
加QQ：99515681  WX：codinghelp  Email: 99515681@qq.com
