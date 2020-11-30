# Web Technologies
### Packet switching
In [telecommunications](https://en.wikipedia.org/wiki/Telecommunication "Telecommunication"), **packet switching** is a method of grouping [data](https://en.wikipedia.org/wiki/Data_(computing) "Data (computing)") that is transmitted over a digital [network](https://en.wikipedia.org/wiki/Telecommunications_network "Telecommunications network") into [packets](https://en.wikipedia.org/wiki/Network_packet "Network packet"). Packets are made of a [header](https://en.wikipedia.org/wiki/Header_(computing) "Header (computing)") and a [payload](https://en.wikipedia.org/wiki/Payload_(computing) "Payload (computing)"). Data in the header is used by networking hardware to direct the packet to its destination, where the payload is extracted and used by [application software](https://en.wikipedia.org/wiki/Application_software "Application software"). 
You can check your nodes along the way with either $ tracepath or $ traceroute6:

### Tracepath
[Read](https://www.pcwdld.com/traceroute)
TTL: Time to live. Tells a package how many hops it is allowed to make. 

If the hops are exceeded, the package gets sent back with details about the time it took and the IP of the last node it reached. Tracepath sends packages with increasing TTL, starting from 1 and collects the data that has been sent back.
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTQ3Njk2MDE2Nl19
-->