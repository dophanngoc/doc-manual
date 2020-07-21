# temp
#!/usr/bin/python3
import signal
import time


def scheduler(arg1, args2):
    print(time.time())

signal.signal(signal.SIGALRM, scheduler)
signal.setitimer(signal.ITIMER_REAL, 1, 1)
print('Start')
while True:
    print("PRINT")
    time.sleep(5)


@startuml
scale 2000 width
scale 1200 height

start --> add_new_sensor
start --> mcast_regularly
mcast_regularly : 5min
mcast_regularly --> mcast_regularly : transfer_gw_ipaddr
add_new_sensor --> add_new_sensor : transfer_gw_ipaddr
state Degu_GW {
  state "commissioner succeeded" as long1
  threading --> threading : every 5min
  long1 : threading wait 10 sec
  long1 --> long1
  long1--> create_UDP_socket

  threading --> create_UDP_socket
  create_UDP_socket : mcastIP,mPort
  create_UDP_socket --> send_Multicast
  send_Multicast : included GW ipAddr
  send_Multicast --> send_Multicast : 3 shots
}

left to right direction

Degu_GW --> New_degu : 2.4Ghz Wireless

top to bottom direction

state New_degu {
  state "join succeeded" as long2
  Openthread_tasklet1 --> Openthread_tasklet1
  long2 --> initialize_UDP
  initialize_UDP --> create_socket
  create_socket : mIpv6, mPort
  create_socket --> handler_UDP_comming
  handler_UDP_comming --> handler_UDP_comming : interrupt
  handler_UDP_comming --> get_GW_Addr
  get_GW_Addr : put to COAP socket
  get_GW_Addr --> get_GW_Addr
}
@enduml

@startuml
scale 2000 width
scale 1000 height
hide empty description
state Degu {
  state "PowerON" as long0
  long0 : replug / reset
  long0 --> create_socket : start zephyr
  long0 --> initUdp
  initUdp --> openthread_Tasklet : openthread task
  openthread_Tasklet --> openthread_Tasklet
  create_socket --> sendUDP : mcast
  state "UDP receive handler" as long3
  long3 --> get_GW_addr
  get_GW_addr : put to COAP socket
}

sendUDP --> Router : Request gwip

state Degu_GW {
  state "Create socket Sever" as long1
  long1 --> long1 : New Data
  long1 --> udpHander_Rereceive : Start
  udpHander_Rereceive --> udpHander_Rereceive
  Router --> udpHander_Rereceive : Request gwip
  udpHander_Rereceive --> get_degu_addr
  get_degu_addr --> send_unicast
  send_unicast -> Router : response ipaddr
  Router --> long3 : response ipaddr
}
@enduml

#!/usr/bin/python3

import socketserver
import socket
import struct

HOST, PORT = "fe80::c85c:3e0a:ef23:4fb3", 54321
intf_name = "wlan1"
class MyUDPHandler(socketserver.BaseRequestHandler):
    def handle(self):
        data = self.request[0].strip()
        socket = self.request[1]
        print("{} wrote:".format(self.client_address[0]))
        print(data)
        socket.sendto(data.upper(), self.client_address)

class ThreadingUDPServer(socketserver.UDPServer):
    address_family = socket.AF_INET6


if __name__ == "__main__":
    #intf_index = socket.if_nametoindex(intf_name)
    #rxsock = socket.socket(socket.AF_INET6, socket.SOCK_DGRAM, socket.IPPROTO_UDP)
    #req = struct.pack("=16si", socket.inet_pton(socket.AF_INET6, mcast_address), intf_index)
    #rxsock.setsockopt(socket.IPPROTO_IPV6, socket.IPV6_ADD_MEMBERSHIP, req)
    rxsock = ThreadingUDPServer(('::', PORT), MyUDPHandler)
    rxsock.serve_forever()
    print("OK")

https://www.youtube.com/watch?v=FsmUquyfd7o
https://www.youtube.com/watch?v=OKc_V7C7jjQ



#!/usr/bin/python3

import socketserver
import socket
import struct
import threading

HOST, PORT = "fe80::c85c:3e0a:ef23:4fb3", 54321
intf_name = "wlan1"
class MyUDPHandler(socketserver.BaseRequestHandler):
    def handle(self):
        data = self.request[0].strip()
        socket = self.request[1]
        print("{} wrote:".format(self.client_address[0]))
        print(data)
        socket.sendto(data.upper(), self.client_address)

class ThreadingUDPServer(socketserver.UDPServer):
    address_family = socket.AF_INET6


if __name__ == "__main__":

    address = ('localhost', 0) # let the kernel give us a port
    server = socketserver.UDPServer(address, MyUDPHandler)
    ip, port = server.server_address # find out what port we were given

    # Connect to the server
    s = socket.socket(socket.AF_INET6, socket.SOCK_STREAM)
    s.connect((ip, port))

    # Send the data
    message = 'Hello, world'
    print('Sending : %s' % message)
    len_sent = s.send(message)

    # Receive a response
    response = s.recv(len_sent)
    print('Received: "%s"' % response)

    # Clean up
    s.close()
    server.socket.close()

    #intf_index = socket.if_nametoindex(intf_name)
    #rxsock = socket.socket(socket.AF_INET6, socket.SOCK_DGRAM, socket.IPPROTO_UU
DP)
    #req = struct.pack("=16si", socket.inet_pton(socket.AF_INET6, mcast_address))
, intf_index)
    #rxsock.setsockopt(socket.IPPROTO_IPV6, socket.IPV6_ADD_MEMBERSHIP, req)
    #rxsock = ThreadingUDPServer(('::', PORT), MyUDPHandler)
    #rxsock.serve_forever()
    #print("OK")
