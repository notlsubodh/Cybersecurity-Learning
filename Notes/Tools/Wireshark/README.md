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

# Main window navigation
Packet list and detail navigation can be done entirely from the keyboard. Table 3.1, “Keyboard Navigation” shows a list of keystrokes that will let you quickly move around a capture file.
Table 3.1. Keyboard Navigation

Accelerator	Description
Tab or Shift+Tab --> Move between screen elements, e.g., from the toolbars to the packet list to the packet detail.

↓ --> Move to the next packet or detail item. Holding the key down will move more quickly.

↑ --> Move to the previous packet or detail item. Holding the key down will move more quickly.

Ctrl+↓ or F8 --> Move to the next packet, even if the packet list isn’t focused.

Ctrl+↑ or F7 --> Move to the previous packet, even if the packet list isn’t focused.

Ctrl+. --> Move to the next packet of the conversation (TCP, UDP or IP).

Ctrl+, --> Move to the previous packet of the conversation (TCP, UDP or IP).

Alt+→ or Option+→ (macOS) -->Move to the next packet in the selection history.

Alt+← or Option+← (macOS) -->Move to the previous packet in the selection history.

← --> In the packet detail, closes the selected tree item. If it’s already closed, jumps to the parent node.

→ --> In the packet detail, opens the selected tree item.

Shift+→ --> In the packet detail, opens the selected tree item and all of its subtrees.

Ctrl+→ --> In the packet detail, opens all tree items.

Ctrl+← --> In the packet detail, closes all tree items.

Backspace --> In the packet detail, jumps to the parent node. 

Return or Enter --> In the packet detail, toggles the selected tree item.


View → Internals → Keyboard Shortcuts will show a list of all shortcuts in the main window. Additionally, typing anywhere in the main window will start filling in a display filter.

# The menu
Wireshark’s main menu is located either at the top of the main window (Windows, Linux) or at the top of your main screen (macOS).
 "Note
Some menu items will be disabled (greyed out) if the corresponding feature isn’t available. For example, you cannot save a capture file if you haven’t captured or loaded any packets."


<img width="536" height="28" alt="image" src="https://github.com/user-attachments/assets/99d04be1-a82d-452e-917c-72fdf6c79dcd" />

The main menu contains the following items:

File
This menu contains items to open and merge capture files, save, print, or export capture files in whole or in part, and to quit the Wireshark application. 
<img width="1066" height="792" alt="image" src="https://github.com/user-attachments/assets/c536a2ba-235b-46fc-9b0c-a25bf8a8dbac" />

Edit
This menu contains items to find a packet, set a time reference, or mark one or more packets, handle configuration profiles, and set your preferences; (cut, copy, and paste are not presently implemented).
<img width="1066" height="792" alt="image" src="https://github.com/user-attachments/assets/80728a16-9845-404a-b91d-a2f152215266" />

View
This menu controls the display of the captured data, including colorization of packets, zooming the font, showing a packet in a separate window, expanding and collapsing trees in packet details, …​. 
<img width="1088" height="777" alt="image" src="https://github.com/user-attachments/assets/754dff53-e956-4c3f-80ba-00ae6de428aa" />

Go
This menu contains items to go to a specific packet. 
<img width="1066" height="792" alt="image" src="https://github.com/user-attachments/assets/90ca064f-13d7-4515-914d-e6ddc29576d8" />

Capture
This menu allows you to start and stop captures and to edit capture filters. 
<img width="1066" height="792" alt="image" src="https://github.com/user-attachments/assets/2bda796a-156e-4426-9473-8b57d7734f97" />

Analyze
This menu contains items to manipulate display filters, enable or disable the dissection of protocols, configure user specified decodes and follow a TCP stream. 
<img width="911" height="684" alt="image" src="https://github.com/user-attachments/assets/74e2d4ef-99c0-4514-9038-fa4d1f1be802" />

Statistics
This menu contains items to display various statistic windows, including a summary of the packets that have been captured, display protocol hierarchy statistics and much more. 
<img width="1066" height="792" alt="image" src="https://github.com/user-attachments/assets/9f482baa-4297-4074-9c39-5969812bd1b0" />

Telephony
This menu contains items to display various telephony related statistic windows, including a media analysis, flow diagrams, display protocol hierarchy statistics and much more. 
<img width="796" height="694" alt="image" src="https://github.com/user-attachments/assets/5addfc99-e398-4873-bdb2-34eb6661e957" />

Wireless
This menu contains items to display Bluetooth and IEEE 802.11 wireless statistics.

Tools
This menu contains various tools available in Wireshark, such as creating Firewall ACL Rules. 
<img width="992" height="729" alt="image" src="https://github.com/user-attachments/assets/00ac1137-9803-45f7-bbef-e48c8101f642" />

Help
This menu contains items to help the user, e.g., access to some basic help, manual pages of the various command line tools, online access to some of the webpages, and the usual about dialog. 
<img width="1066" height="792" alt="image" src="https://github.com/user-attachments/assets/3cb60796-520b-4f9f-9bb5-867bdbbcb3ea" />



[Tip]	Shortcuts make life easier
Most common menu items have keyboard shortcuts. For example, you can press the Control and the K keys together to open the “Capture Options” dialog.

# The “Main” Toolbar
The main toolbar provides quick access to frequently used items from the menu. This toolbar cannot be customized by the user, but it can be hidden using the View menu if the space on the screen is needed to show more packet data.

Items in the toolbar will be enabled or disabled (greyed out) similar to their corresponding menu items. For example, the image below shows the main window toolbar after a file has been opened. Various file-related buttons are enabled, but the stop capture button is disabled because a capture is not in progress.

 The “Main” toolbar
<img width="1026" height="48" alt="image" src="https://github.com/user-attachments/assets/df66af9b-bf8b-40e9-9c9f-affc90b38602" />

# | Icon | Toolbar Item | Menu Item | Description |
|---|------|-------------|-----------|-------------|
| 1 | capture start | Start | Capture → Start | Starts capturing packets with the same options as the last capture or the default options if none were set. |
| 2 | capture stop | Stop | Capture → Stop | Stops the currently running capture. |
| 3 | capture restart | Restart | Capture → Restart | Restarts the current capture session. |
| 4 | capture options | Options… | Capture → Options… | Opens the "Capture Options" dialog box. |
| 5 | document open | Open… | File → Open… | Opens the file open dialog box to load a capture file. |
| 6 | capture file save | Save As… | File → Save As… | Saves the current capture file. Shows "Save" icon if a temporary capture is open. |
| 7 | capture file close | Close | File → Close | Closes the current capture. Prompts to save if unsaved. |
| 8 | capture file reload | Reload | View → Reload | Reloads the current capture file. |
| 9 | edit find | Find Packet… | Edit → Find Packet… | Find a packet based on different criteria. |
| 10 | go previous | Go Back | Go → Go Back | Jump back in packet history. Hold **Alt** (Option on macOS) to go back in selection history. |
| 11 | go next | Go Forward | Go → Go Forward | Jump forward in packet history. Hold **Alt** (Option on macOS) to go forward in selection history. |
| 12 | go jump | Go to Packet… | Go → Go to Packet… | Go to a specific packet. |
| 13 | go first | Go To First Packet | Go → First Packet | Jump to the first packet of the capture file. |
| 14 | go last | Go To Last Packet | Go → Last Packet | Jump to the last packet of the capture file. |
| 15 | stay last | Auto Scroll in Live Capture | Go → Auto Scroll in Live Capture | Auto scroll packet list during a live capture (toggle on/off). |
| 16 | colorize packets | Colorize | View → Colorize Packet List | Colorize the packet list (toggle on/off). |
| 17 | aggregation | Aggregate Packets | View → Aggregate Packets | Activates Aggregation View, displaying frames grouped by selected field values. |
| 18 | zoom in | Zoom In | View → Zoom In | Increase the font size of packet data. |
| 19 | zoom out | Zoom Out | View → Zoom Out | Decrease the font size of packet data. |
| 20 | zoom original | Normal Size | View → Normal Size | Reset zoom level to 100%. |
| 21 | resize columns | Resize Columns | View → Resize Columns | Resize columns so content fits. |
| 22 | reset layout 2 | Reset Layout | View → Reset Layout | Reset layout to default size. |   


