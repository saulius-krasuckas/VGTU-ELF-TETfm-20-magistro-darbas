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
    Išsprendžiama:
    > As the de facto load balancing scheme in data center, Equal Cost Multipath (ECMP) [3] randomly sends each flow to one of the parallel paths by hash function. Though ECMP is very simple, it suffers from well-known performance problems such as hash collisions and low link utilization. Random Packet Spraying (RPS) [4] is a packet-level load balancing scheme that sprays each packet to a random path to obtain high link utilization, while also leading to packet reordering and throughput loss. Compared with the flow-level and packet-level load balancing schemes, MultiPath TCP (MPTCP) uses parallel subflows as the rerouting granularity to achieve better tradeoff in minimizing packet reordering and increasing link utilization [5].  
    > 
    Trūkumai: 
    > Unfortunately [...] the number of subflows has significant impact on MPTCP performance. On the one hand, if the number of subflows is small, the network resources of multiple paths are wasted, resulting in suboptimal flow completion time and even hot spots in congested paths. On the other hand, if the number of subflows is large, the congestion window of each subflow becomes very small, easily leading to timeout (i.e., full window loss) and high tail latency under heavy congestion [6–10].  
    > 
  * https://network-insight.net/2016/12/multipath-tcp/
    Išsprendžiama:
    > Next generation leaf and spine data centre networks are built with Equal-Cost Multipath (ECMP). [...] For one endpoint to communicate to another, a TCP flow is placed on a single link, not spread over multiple links. As a result, single-path TCP collisions may occur, reducing the throughput available to that flow. This is commonly seen for large flows and not small mice flows.  
    > In a data centre when a server starts a TCP connection it gets placed on a path and stays there. With MPTCP instead of using a single path per connection you could use many subflows per connection. If some of those subflows get congested, you just don’t send over that particular subflow improving traffic fairness and bandwidth optimisations.  
    > The default behaviour of spreading traffic through a LAG or ECMP next hops is based on the hash-based distribution of packets. An array of buckets is created, and each outbound link is assigned to one or more buckets. Fields are taken from the outgoing packet header such as source-destination IP address / MAC address and hashed based on this endpoint identification. A bucket is selected by the hash and the packet is queued to the interface that is assigned that bucket.  
    > The issue here is that the load balancing algorithm does not take into account interface congestions or packet drops. With all mice flows this is fine but once you mix mice and elephant flows together your performance will suffer. An algorithm is needed to identify congested links and then reshuffle the traffic. A good use for MPTCP is when you have a mix of mice and elephant flows.   
    > 
    Trūkumai: 
    > Generally, MPTCP does not improve performance for environments with only mice flows.  
    > With small files say 50KB MPTCP offers the same performance as regular TCP. As the file size increases MPTCP usually has the same results as link bonding. The benefits of MPTCP come to play when files are very big (300 KB ). At this level,  MPTCP outperforms link bonding as the congestion control can efficiently balance the load better over the links.  
    > 
 
  * .

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

* 
