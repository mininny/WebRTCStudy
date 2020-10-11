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

### Frame Rate
Number of frames per second that are sent and received. 

Typical video call aims for 30 frames per second, but may drop depending on the network condition, processing capabilities, and available bandwidth. 

Frame rate fluctuates throughout time due to packet loss. 

### FEC (Forward Error Correction)
FEC is used to overcome packet loss in WebRTC.

In FEC, media packets are duplicated and sent multiple times across the network. This ensures that the media stream will continue properly even if some packets are not received. 

> FEC is included in WebRTC as part of Opus Codec. 

### ICE
Interactive Connectivity Establishment.

ICE is used to do NAT traversal in WebRTC. It performs connectivity checks and use appropriate connection method to traverse NAT restrictions. 

ICE collects all available candidates(local, reflexive, relayed) and perform connectivity check with the peer using all possible candidates. 

While ICE allows peers to connect over different network conditions, it may take few seconds. To overcome this, WebRTC introduced [trickle ICE](#trickle-ICE)

### ice-lite
Minimal version of [ICE](#ICE) specification. Only have to answer incoming STUN binding requests and dact as a controlled entity in the ICE process. 

Easy to implement, and popular among implementations of SFUs and other media servers. 

### ICE-TCP
ICE-TCP is a mechanism where media is sent over TCP instead of TURN.

This removes the need to relay the media in the TURN server, and the direct connection delivers the media faster. It also removes the restriction of NAT or firewalls. 

### Jitter
Jitter is the variation in the time between data packets arriving, due to various network conditions such as congestion. 

This makes the sequence of the media packets out of order, making the media weird.

Because media are time-sensitive, the media packets are collected and reordered in order to match the original signal. This is handled by the [Jitter buffer](#jitter-buffer).

### Jitter buffer
Jitter buffer handles [packet reordering](#packet-reordering) and [jitter](#jitter). 

Jitter buffer collects and stores incoming media packets, and decides when to pass them into the decoder or player based on:
- the type of the packet
- the packet that is waited for
- time required to play the media

### Latency
Latency is the time for a process to complete. 

Latency can be measured in many different areas, such as: 
- time to encode/decode a media frame
- time to send the packets through the network 
- time for Jitter Buffer to process packets
- etc

Usually in WebRTC, it's the time between a peer sending a media packet and an opposite pear receiving the media packet. 

### Lip Synchronization
Synchronization of audio and video trakcs on the receiver end. 

In WebRTC, the raw media timestamped, encoded, and sent over the network. During this process, the audio and video are handled separately to improve network efficiency. 

When the media packets are received, they are sent to [jitter buffer](#jitter-buffer). The audio/video are then delayed as they are matched with the different media track with same timestamp.

### Media Engine
Media Engine is part of software that handles the media processing. It handles:
- integrating voice/video codecs
- integrating peripherals and devices such as microphone, camera, speaker, and display
- handling network problems like packet loss, packet reordering, and jitter
- handling acoustic echo
- optimizing media quality. i.e. noise reduction
- implementing network transport protocol, such as RTP

> WebRTC is a media engine with standardized Javascript API embedded into web browsers.

### Mesh
Mesh is multipoint architecture where every participant sends and receives media to/from all other participants. 

> Mesh is usually scalable to 4-6 participants for video call at most. 

Mesh is simple and require little backend infrastructure. However, it can not scale to large number of participants and requires a lot of bandwidth from all participants. 

### Mixing
Mixing is multipoint architecture where every participants sends/receives single media stream from a central server. This central server mixes all(or some) of the streams it receives. 

> Mixing is done by a [MCU](#mcu-multipoint-conferencing-unit) server. 

Mixing requires little power from the client side. However, it requires heavy workload for the server as it needs to decode, layout, and re-encode the media from many participants. 

### MCU (Multipoint Conferencing Unit)
MCU connects to multiple participants in a voice/video session, and implements mixing architecture. Due to this nature, they are expensive and require a lot of processing power per session. 

### NACK (Negative Acknowledgement)
NACK is error resiliency mechanism where a receiver sends a message indicating that it hasn't received a specific packet. 

NACK is sent over [RTCP](#rtcp), and wil be decided if a retransmission of the lost packet is available and useful.

### Packet Loss
Packet loss occurs when sent packets of datas fail to reach their destination. This is part of network design. Its reasons are:
- Corruption. Packet being corrupted and misdelivered.
- Congestion. Load in network may delay or drop some packets. 

Packet loss is dealt with three methods: 
- [Concealment](#concealment)
- [Retransmission](#rtx-retransmission)
- [Forward Error Correction](#fec-forward-error-correction)

### Packet Loss Concealment
PLC is mechanism built to overcome packet loss problems in WebRTC.

Using different sets of algorithms, PLC aims to fill the empty periods of media packets without affecting the quality of audio/video.

PLC is not standardized and is left for the implementor of media engine and codec.

### Packet Reordering
Packet Reordering is the process in which the packets are reordered to the way they were sent. 

While WebRTC's transport protocol does not handle packet reordering, the [RTP](#rtp) contains mechanism for packet reordering. 

### Transcoding
Process of translating one codec to another. It requires decoding the data and re-encoding with another codec. 

Transcoding may be necessary when different codec is required or has to be streamed to incompatible devices. 

### Trickle ICE
Trickle ICE is optimization of ICE. 

Instead of waiting for all possible ICE candidates (which may take several roundtrips), trickle ICE parallelizes the process. 

Trickle ICE sends the ICE candidate as soon as they become available, reducing the whole process time. 

### RTX (Retransmission)
Retransmission is used to resend packets that were not successfully delivered to the recipient. 

While retransmission is usually useless for real-time media stream, there are some instances where it may be necessary. 

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

### SDP
Session Description Protocol. [RFC 4566](https://tools.ietf.org/html/rfc4566)

SDP is used to negotiate WebRTC session's parameters. It contains information about the multimedia communication sesion. SDP should be handled by the application because WebRTC does not handle signaling. 

SDP contains: 
- rtpmap
- BUNDLE
- rtcp-mux
- setup
- fmtp
- ssrc
- [DTLS-SRTP](#dtls-srtp)
- ICE

### SDP Munging
SDP Munging, or SDP mangling, is the process of modifying the SDP message before passing it to WebRTC. 

You can mangle SDP of `answer` and `offer` before it is handed to `setLocalDescription` or `setRemoteDescription`.

SDP munging is used to control and modify the behavior of WebRTC. 

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

### VBR (Variable Bit Rate)
Encoder generating different amount of bits per second based mainly on the content. The content may affect the latency, therefore the network.  

Do not adhere to bandwidth constraints and may cause congestion and latency. Popular with screen sharing encoding with static content.  

> Realtime media usually use [CBR](#cbr-constant-bit-rate)
