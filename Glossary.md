## Glossary from [WebRTC Glossary](https://www.webrtcglossary.com)

### API
Application Programming Interface. 

Denotes a set of well defined functions or procedures that is invoked to generate a given outcome. 

### AEC (Acoustic Echo Cancellation)
Sometimes, the mic of a user can catch sound from the its own speaker, causing the voice to reverbrate and create acoustic echo. 

AEC aims to cancel such echo and is very important for VoIP solutions. AEC is built into WebRTC.

### Bitrate
Number of bits that can be sent or received per time unit. This can determine the quality of the media content. 

Maximum bitrate may be capped by the bandwidth available. See [BWE](#bwe-bandwidth-estimation)

### BWE (Bandwidth Estimation)
Mechanism used to decide how much bandwidth is available for a given session. 

Because the availability of bandwidth may  change throughout a call, estimating and adjusting the quality of the media is helpful to efficiently use the allocated bbandwidth. 

> Based on heuristics that model the network behavior. Varies between different network types. 

### Congestion
Happens when the packets cant be sent to the next network fast enough, and get queued in the router's internet buffer. Leads to increased latency and jitter, and possibly packet drops. 

> [BWE](#bwe-bandwidth-estimation) is used to prevent congestion, along with the limitation of bitrate. 

### CBR (Constant Bit Rate)
Encoder generating same number of bits for a short period of time (<1 sec).

Work well with known and limited network bandwidth. Common in live video sessions where latency must remain low. 

> Larger stored recordings usually use [VBR](#vbr-variable-bit-rate)

### Codec
Piece of software that encodes and decodes a digital stream of media. 
- Encoding: takes raw digital data of media and compreses/encodes it to make it easier to send/store. 
- Decoding : takes encoded data and decodes it to make it possible to play the media. 

Codecs differ by the level of compression, CPU power, speed of operation, and losslessness. They can be implemented via software, hardware, or both. 

### DTMF
Dual Tone Multi Frequency.

Signal(frequency) that is generated when you press a telephone's number keys. These digits are used to dial numbers.

DTMF signaling can happen in-band or out-of-band.

### DTLS
Datagram Transport Layer Security. Essentially, it's UDP with security in mind. 

DTLS is similarly based on TLS. 

### DTLS-SRTP
Key exchange mechanism for WebRTC. Uses DTLS to exchange keys for the SRTP media transport. 

### Data Channel
Channel that sends arbitrary data directly between peers. 

Data Channel uses [SCTP](#sctp) as its transport protocol. 

### VBR (Variable Bit Rate)
Encoder generating different amount of bits per second based mainly on the content. The content may affect the latency, therefore the network.  

Do not adhere to bandwidth constraints and may cause congestion and latency. Popular with screen sharing encoding with static content.  

> Realtime media usually use [CBR](#cbr-constant-bit-rate)

### RTP
Real-time Transport Protocol. It is designed for sending and receiving real time media. 

RTP is built on top of UDP with the focus on low latency. It contains timestamp and a sequence number to prevent network issues.   

RTP is composed of: 
- Packet Header.
- Payload Header. 
- Payload data unit.

### RTCP
Real-time Transport Control Protocol. 

RTCP is a control mechanism for RTP that is used to send statistics reports and flow control messages. This allows the feedback of peers and deductions of the environment, such as the network status. 

### SRTP 
Secure RTP. It adds security to RTP, offering a secure and private mechanism to send and receive data with encryption. 

The use of [DTLS-SRTP](#dtls-srtp) allows for safe and private exchange of data through DTLS. The data is encrypted and authenticated by the shared secret.  

SRTP provides: 
- Integrity. 
- Authentication.
- Privacy. 

> RTP headers are not encrypted.
> 
> While SRTP allows for NULL encryption mechanism, such usage is disabled in WebRTC.

### SRTCP
SRTCP is Secure version of RTCP. This is used alongside SRTP.

### SCTP
Stream Control Transmission Protocol. Essentially, it's a hybrid of UDP and TCP. 

SCTP is: 
- Connection oriented. 
- Optional reliability. Reliability of the delivery can be deteremined by the implementer. 
- Optional ordering. Ordering of packets can be determined by the implementer. 
- Message oriented. Ensures that messages are properly parsed.
- Flow control. Flow control mechanisms prevent network congestion. 

> SCTP is used for [Data Channel](#data-channel) in WebRTC.

### TCP
Transmission Control Protocol. Low Level IP Protocol that is used to send reliable data stream.

TCP provides the following guarantees:
- Connection oriented. You have to connect to the remote entity before sending/receiving data. 
- Reliability. The sent data is guaranteed to be delivered, or get connection closed. 
- Ordering. Data sent will be received in the same order it was sent. 
- Single stream. Data sent over TCP is single long stream of data that can be split into messages. 
- Flow control mechanism. TCP will change the bandwidth depending on the network condition. 

### TLS
Transport Layer Security, or olderly known as SSL (Secure Sockets Layer). Essentially, it's TCP with security in mind. 

TLS provides the following:
- Authentication. Use certificates from client and/or server, the connection can authenticate with the credential of the peer. 
- Integrity. MAC (Message authentication code, aka. checksum) is genearted for each message sent, and can be checked to ensure the integrity of the data. 
- Privacy. TLS encrypts the data using public key cryptography. It achieves this using TLS handshake. 

> See [DTLS](#dtls) for the UDP variant of TLS.

### UDP
User Datagram Protocol. Low Level IP Protocol used to send data packets. 

UDP provides the following guarantees:
- Connectionless. UDP socket does not require established conection. 
- No Ack. UDP does not guarantee the delivery of the packet, and no acknowledgement of receipt. 
- No inherent ordering. Packets sent may be received in different order than it was sent. 
- Single Units. UDP messages are small enough to fit a single packet. 
- No Flow control mechanism. Congestion leads to packet loss. 

> UDP is useful in scenarios where low latency is important. It is used in RTP and SRTP. 
