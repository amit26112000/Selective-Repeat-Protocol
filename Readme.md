# ACN-Selevtive-Repeat-Protocol
Implementation of a modified version of a Selective Repeat (SR) sliding window protocol for the file transfer.

# Protocol Description:
1. Client and server establish a connection over UDP sockets.
2. Client sends value of window_size (W) (i.e., amount of packets can be send simultaneously without an ACK) to server (receiver) over socket.
3. Client reads data from a file named as in.txt into a buffer. (create a file with sufficient data to send several packets)
4. Client calculate the total number of packets (N) to be send to transfer the file.
5. Client sends W amount of packets to the server one by one. After that, it runs a Timer (RTO). Each packet comprises of packet_no, packet_size and the data (i.e., chunk of file-data equals to packet_size).
6. Server randomly discards packet(s) based on a loss rate function to emulate the packet drop scenario. Such packets do not considered as received at server.
7. Server sends an individual ACK packet for each successfully received packet. Server does not send any ACK packet for discarded packets in (Step-6). The ACK packet comprises of ack_no (corresponding to packet_no received from client).
8. Server copies the data received from client into a file named as out.txt. Data must be copied to the file in order. (At the end in.txt and out.txt must have similar contents (byte by byte.)
9. Client slides its window (i.e., change the base) every time after receiving an in-order ACK and sends new packets accordingly.
10. If a Timer expires for a packet then sender resends that packet and run Timer for next outstanding packet (i.e., a packet which has been sent but ACK is not received yet).
11. Client repeats the Steps 5-10 to send the entire file to the server.
12. After entire file is transferred, client and server close the connection.

Sample Output:<br>
a) Client program displays the packet_no of each data packet transmitted including retransmitted packets and ack_no of each ACK packet received. Also, it displays a message to indicate for which packet no. Timeout (RTO) occurred. A sample output is shown below:

SEND PACKET 13 : BASE 11  <br>
RECEIVE ACK 11 : BASE 12  <br>
TIMEOUT 12<br>
SEND PACKET 12 : BASE 12<br>
SEND PACKET 14 : BASE 12<br>
RECEIVE ACK 12 : BASE 13<br>

b) Server program displays packet_no of each packet received along with the information whether it is Dropped or Accepted. Also, displays ack_no of each ACK packet transmitted and window BASE value. A sample output is shown below:

RECEIVE PACKET 12 : DROP : BASE 11<br>
RECEIVE PACKET 13 : ACCEPT : BASE 11<br>
RECEIVE PACKET 12 : ACCEPT : BASE 12<br>
SEND ACK 12<br>
