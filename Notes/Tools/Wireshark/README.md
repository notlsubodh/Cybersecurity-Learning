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
