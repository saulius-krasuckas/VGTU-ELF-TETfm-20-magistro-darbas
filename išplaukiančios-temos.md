* Kokie yra DCN (Data Center Networks) tipai?
  * https://www.diva-portal.org/smash/get/diva2:1164941/FULLTEXT01.pdf#page=10
    > 2 Overview of Data Center Architectures  
    > 2.1 Data Center Network Topologies  
    > 3 Data Center Networking  
    > 3.1 Data Center Network  
    > 3.2 Data Center Networking  
  * Kurį DCN tipą (ar kelis) tirsime / emuliuosime?

* Ką išsprendžia MPTCP ir kokie jo trūkumai?  
  (Juos turbūt ir verta tirti)
  * https://journalofcloudcomputing.springeropen.com/articles/10.1186/s13677-020-00160-3  
    Išsprendžia:
    > As the de facto load balancing scheme in data center, Equal Cost Multipath (ECMP) [3] randomly sends each flow to one of the parallel paths by hash function. Though ECMP is very simple, it suffers from well-known performance problems such as hash collisions and low link utilization. Random Packet Spraying (RPS) [4] is a packet-level load balancing scheme that sprays each packet to a random path to obtain high link utilization, while also leading to packet reordering and throughput loss. Compared with the flow-level and packet-level load balancing schemes, MultiPath TCP (MPTCP) uses parallel subflows as the rerouting granularity to achieve better tradeoff in minimizing packet reordering and increasing link utilization [5].  
    > 
    Trūkumai: 
    > Unfortunately [...] the number of subflows has significant impact on MPTCP performance. On the one hand, if the number of subflows is small, the network resources of multiple paths are wasted, resulting in **suboptimal flow completion time** and even **hot spots in congested paths**. On the other hand, if the number of subflows is large, the congestion window of each subflow becomes very small, easily **leading to timeout** (i.e., full window loss) and **high tail latency** under heavy congestion [6–10].  
    > 
  * https://network-insight.net/2016/12/multipath-tcp/  
    Išsprendžia:
    > Next generation leaf and spine data centre networks are built with Equal-Cost Multipath (ECMP). [...] For one endpoint to communicate to another, a TCP flow is placed on a single link, not spread over multiple links. As a result, single-path TCP collisions may occur, reducing the throughput available to that flow. This is commonly seen for large flows and not small mice flows.  
    > 
    > In a data centre when a server starts a TCP connection it gets placed on a path and stays there. With MPTCP instead of using a single path per connection you could use many subflows per connection. If some of those subflows get congested, you just don’t send over that particular subflow improving traffic fairness and bandwidth optimisations.  
    > 
    > The default behaviour of spreading traffic through a LAG or ECMP next hops is based on the hash-based distribution of packets. An array of buckets is created, and each outbound link is assigned to one or more buckets. Fields are taken from the outgoing packet header such as source-destination IP address / MAC address and hashed based on this endpoint identification. A bucket is selected by the hash and the packet is queued to the interface that is assigned that bucket.  
    > 
    > The issue here is that the load balancing algorithm does not take into account interface congestions or packet drops. With all mice flows this is fine but once you mix mice and elephant flows together your performance will suffer. An algorithm is needed to identify congested links and then reshuffle the traffic. A good use for MPTCP is when you have a mix of mice and elephant flows.   
    > 
    Trūkumai: 
    > Generally, MPTCP **does not improve performance** for environments **with only mice flows**.  
    > 
    > With small files say 50KB MPTCP offers the same performance as regular TCP. As the file size increases MPTCP usually has the same results as link bonding. The benefits of MPTCP come to play when files are very big (300 KB ). At this level,  MPTCP outperforms link bonding as the congestion control can efficiently balance the load better over the links.  
    > 
 
  * https://slideplayer.com/slide/17007643/  
    `TODO` https://arxiv.org/pdf/1707.00322.pdf  
    `TODO` https://github.com/mkheirkhah/amp  
    Išsprendžia:
    >
    > ECN-based multipath schemes seem to provide a good balance between the latency-throughput trade-off  
    > 
    Trūkumai:
    > 
    > Problems with ECN-capable variant of MPTCP
    > * TCP Incast  
    >   Well-studied topic for TCP (not really for MPTCP)  
    >   Each subflow maintains a separate congestion window  
    >   More subflows, more chance of experiencing a retransmission timeout during an incast episode  
    > * Last Hop Unfairness (LHU)  
    >   We are reporting it for the first time

    > Problem 1: Incast  
    > 
    > * MPTCP and its ECN-capable variants are not robust against the Incast problem…  
    >   * More subflows --> More packets --> Buffer overflow --> Higher chance of RTO in each subflow especially when the congestion window is small  
    >     
    >     Why is that? The problem is very simple, the more subflows is used, the more packets is generated.  
    >     
    >     As a result the switch buffer can easily overflow.  
    >     
    >     Which implies higher chance of RTO in each subflow especially when the congestion window is small (less than 10 packets).

    > Problem 2: Last Hop Unfairness  
    > 
    > Let’s assume:  
    > Propagation delay is zero  
    > Marking threshold (K) at switches sets to 4 packets (K=4)  
    > Minimum congestion window size sets to one packet (cwndmin=1)  
    > 
    > Normal situation  
    > Two single-path flows share the link fairly. Each flow generating two packets per RTT on average  
    > 
    > To explore this problem let’s assume:  
    > PBI: A new arriving… because number of competing flows with minimum cwnd is higher than marking threshold K  
    > LHU: now we can see not only MPTCP cause serious buffer inflation but also it is seriously unfair to competing flows
    > 
    > The LHU leads to severe unfairness and significantly escalates the likelihood of persistent buffer inflation

    > Summary Existing multipath congestion control schemes fail to handle:
    > 
    > * The TCP incast problem that causes **temporal switch buffer overflow** due to synchronized traffic arrival
    > * The last hop unfairness that causes **persistent buffer inflation** and serious **unfairness**

* Kaip DC pritaikomas / kiek sklandus yra / ar įmanomas automatinis srauto Handover-is:
  * https://www.redhat.com/en/blog/understanding-multipath-tcp-networking-highway-future
    > just like a highway clover-leaf interchange where traffic from one highway can merge onto the other with ease, MPTCP allows mobile hosts to hand over traffic from Wi-Fi to cellular, without disrupting the application. [...] MPTCP also dramatically reduces the number of network collisions, which is why you never achieve the full speed of any connection.

* Gal verta ištirti _Network Collision_ pasikeitimus įjungus MPTCP?  
  https://www.redhat.com/en/blog/understanding-multipath-tcp-networking-highway-future
  > just like a highway clover-leaf interchange where traffic from one highway can merge onto the other with ease, MPTCP allows mobile hosts to hand over traffic from Wi-Fi to cellular, without disrupting the application. This is especially important as available bandwidth for wireless connections vary over time and while in motion. And because MPTCP is part of the networking stack, it is transparent to the application. MPTCP also dramatically reduces the number of network collisions, which is why you never achieve the full speed of any connection.


* Energonašumas:
  * `eMTCP` – energy-aware MPTCP  
    A Traffic Burstiness-based Offload Scheme for Energy Efficiency Deliveries in Heterogeneous Wireless Networks  
    https://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.702.3596&rep=rep1&type=pdf
  * angl. _Power efficiency_ vs _Energy efficiency_:  
    https://itpeernetwork.intel.com/power-and-energy-efficiency-double-your-benefit/  
    > Power efficiency is about doing more within a fixed capability  
    > Energy Efficiency is about “making every kWh count”
---
### Kitos mintys

* Pritaikomumas CDNams:  
  * Network Architecture (R02) | IP Multipath – Path Selection&CC  
    Jon Crowcroft  
    https://www.cl.cam.ac.uk/teaching/1213/R02/slides/r02-mpath.ppt#page=2  
    > * IP or Application Layer
    >   * CDN, especially P2P (Torrent or Storm)
    >     already effectively multipath at App
    >   * Current IP routing mainly only corner cases

* WireGuard tunelio panaudojimas MPTCP srauto perdavimui per tuos DC _Middlebox_ mazgus, kurie blokuoja MPTCP žymes/laukus, bet praleidžia UDP.

* Ar DC reikalingas Path menedžeris (dinaminis Routing Table valdymas)?  
  * https://dial.uclouvain.be/memoire/ucl/fr/object/thesis%3A366/datastream/PDF_01/view
    > Simply installing a new Multipath TCP-ready kernel is not enough if you want to use multiple network interfaces at the same time. Indeed, you need to configure routing tables as described on the Multipath TCP’s website [8]
  * http://multipath-tcp.org/pmwiki.php/Users/ConfigureRouting
  > Automatic configuration with "Multihomed-Routing"
  > Kristian Evensen <kristian.evensen@gmail.com> developed a set of scripts that integrate well with existing Network Managers to properly configure the multihomed routing. Check it out at
  * https://github.com/kristrev/multihomed-routing  
    * Ar veikia su `netplan`?
