# Wireshark = a network traffic analyzer. Imagine your computer is sending thousands of tiny envelopes across a network. Wireshark lets you inspect those envelopes:Computer → network → website/server.
• 🌐 Which IP addresses your computer communicates with
• ðŸ“¦ Packets being sent and received
• ðŸ”Œ Protocols such as TCP, UDP, DNS, HTTP, TLS
• ⏱️ When communication happened
• 🐛 Problems such as retransmissions or failed connections
• 🔍 What information is visible when traffic isn't encrypted
Try safely on what you own
Look for DNS, TCP, TLS, HTTP basically filter these using filter (bookmark icon)

# Unencrypted traffic:- If an application sends information without encryption, captured traffic may contain readable application data.
# Encrypted traffic:- with modern HTTPS / TLS, we see our computer→ TLS encrpted traffic → Website. Wireshark can still show useful metadata, such as IPs, packet sizes, timing, and TLS information, but it normally cannot simply display the password.
# Some intended purposes
Here are some reasons people use Wireshark:

* Network administrators use it to troubleshoot network problems
* Network security engineers use it to examine security problems
* QA engineers use it to verify network applications
* Developers use it to debug protocol implementations
* People use it to learn network protocol internals
# Features
The following are some of the many features Wireshark provides:

1.Available for UNIX and Windows.
2.Capture live packet data from a network interface.
3.Open files containing packet data captured with tcpdump/WinDump, Wireshark, and many other packet capture programs.
4.Import packets from text files containing hex dumps of packet data.
5.Display packets with very detailed protocol information.
6.Save packet data captured.
7.Export some or all packets in a number of capture file formats.
8.Filter packets on many criteria.
9.Search for packets on many criteria.
10.Colorize packet display based on filters.
11.Create various statistics.
…​and a lot more!
However, to really appreciate its power you have to start using it.

Figure 1.1, “Wireshark captures packets and lets you examine their contents.” shows Wireshark having captured some packets and waiting for you to examine them.

Figure 1.1. Wireshark captures packets and lets you examine their contents.
<img width="1066" height="792" alt="image" src="https://github.com/user-attachments/assets/fea4068a-5fcd-4fd1-8c53-834b82e89444" />
# Live capture from many different network media
Wireshark can capture traffic from many different network media types, including Ethernet, Wireless LAN, Bluetooth, USB, and more. The specific media types supported may be limited by several factors, including your hardware and operating system.

# Open Source Software
Wireshark is an open source software project, and is released under the GNU General Public License (GPL). You can freely use Wireshark on any number of computers you like, without worrying about license keys or fees or such. In addition, all source code is freely available under the GPL. Because of that, it is very easy for people to add new protocols to Wireshark, either as plugins, or built into the source, and they often do!

# What Wireshark is not
Here are some things Wireshark does not provide:

Wireshark isn’t an intrusion detection system. It will not warn you when someone does strange things on your network that he/she isn’t allowed to do. However, if strange things happen, Wireshark might help you figure out what is really going on.
Wireshark will not manipulate things on the network, it will only “measure” things from it. Wireshark doesn’t send packets on the network or do other active things (except domain name resolution, but that can be disabled).

# Output File Formats
Wireshark can save the packet data in its native file format (pcapng) and in the file formats of other protocol analyzers so other tools can read the capture data.
The following file formats can be saved by Wireshark (with the known file extensions):

pcapng (*.pcapng). A flexible, extensible successor to the libpcap format. Wireshark 1.8 and later save files as pcapng by default. Versions prior to 1.8 used libpcap.
pcap (*.pcap). The default format used by the libpcap packet capture library. Used by tcpdump, _Snort, Nmap, Ntop, and many other tools.
Accellent 5Views (*.5vw)
captures from HP-UX nettl ({asterisktrc0,*.trc1)
Microsoft Network Monitor - NetMon (*.cap)
Network Associates Sniffer - DOS (*.cap,*.enc,*.trc,*.fdc,*.syc)
Cinco Networks NetXray captures (*.cap)
Network Associates Sniffer - Windows (*.cap)
Network Instruments/Viavi Observer (*.bfr)
Novell LANalyzer (*.tr1)
Oracle (previously Sun) snoop (*.snoop,*.cap)
Visual Networks Visual UpTime traffic (*.*)
Symbian OS btsnoop captures (*.log)
Tamosoft CommView captures (*.ncf)
Catapult (now Ixia/Keysight) DCT2000 .out files (*.out)
Endace Measurement Systems’ ERF format capture(*.erf)
EyeSDN USB S0 traces (*.trc)
Tektronix K12 text file format captures (*.txt)
Tektronix K12xx 32bit .rf5 format captures (*.rf5)
Android Logcat binary logs (*.logcat)
Android Logcat text logs (*.*)
Citrix NetScaler Trace files (*.cap)
New file formats are added from time to time.

Whether or not the above tools will be more helpful than Wireshark is a different question ;-)

[Note]	Third party protocol analyzers may require specific file extensions
Wireshark examines a file’s contents to determine its type. Some other protocol analyzers only look at a file’s extension. For example, you might need to use the .cap extension in order to open a file using the Windows version of Sniffer.

