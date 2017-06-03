# SDN--Load-balancer
1. Introduction 

In the 21st century, staying globally connected is the key for most organizations for improving sales and increasing revenue. A high-speed connection, sufficient storage and increased bandwidth is the key factor in developing the networking technology in today’s market.[5] Social networking sites and online web portals for purchasing and selling goods make use of internet servers for delivering reliable information and a secure connection to their customers.The backbone networks of our world have all been connected making use of the BGP (exterior) and OSPF as the interior routing protocol for the various autonomous systems.Dijkstra’s algorithm plays a major role here in finding the most efficient, shortest and least cost path for data delivery.It is this very principle in use while creating a load balancing application on top of the SDN floodlight controller through the Northbound API and using the Openvswitches for network devices utilizing the OpenFlow protocol via the Southbound API of the controller. Creating a virtual topology over Mininet, improvement in the bandwidth, latency and cost of transportation of data have distinctly been displayed along with further scope for the project.

2. Tools Used:
A. Floodlight 
Floodlight is a java-based OpenSDN controller, which works with the OpenFlow protocol to develop a SDN environment. OpenFlow defines a communication protocol using which the control plane (SDN controller) talks to the forwarding plane (switches and routers) to modify and push rules in the network.We have used the Floodlight controller due to its modular design and the ease to use REST APIs to develop an application. The floodlight website offers a variety of REST APIs, which are highly adaptable and gives access to a variety of modules used to make our load balancing application.

B. Mininet 
Mininet is a linux-based network emulator, which builds a virtualized software network.Mininet can be used to quickly create a virtual network running actual kernel in the devices and switches and, software application code on a personal computer. We use mininet for building our topology,which emulates a real scenario. Using this tool, we are able to create different custom topologies by running python scripts and choosing the most suitable topology. Moreover, mininet boots up quickly and has a small footprint. This helps in its efficiency.

C. Iperf
In this project we use Iperf to create a data-stream of ICMP packets to analyze the bandwidth between the two selected hosts. Beyond this, one can create TCP or UDP packets to measure the throughput and losses using Iperf network testing tool. It allows the user to optimize or test a network by setting various parameters like datagram size.Typical Iperf output contains a time-stamped report of the amount of data transferred and the throughput measured. We have used Iperf to ping the data from one host to another and in return we obtain the bandwidth with a timestamp attached to it.

D. Dijkstras Algorithm and Networkx
Dijkstra’s Algorithm is used to find the shortest path between two nodes. In this application, a graph is formed with help of networkx library in python. The nodes represent the vertices of this graph and the links from nodes to other nodes represent the edges of the graph. Based on this graph, Dijkstra’s Algorithm is used to source node and find the shortest path to every node from this source.[9] Thus, we obtain a shortest path tree for the entire network and we can hence choose the shortest path from one segment of the topology. This is used to calculate the transmission rates, which gives the link cost. With the help of all of these decisions, the best path is obtained.


3. IMPLEMENTATION DETAILS
A. Infrastructure details:
We have used Floodlight as our SDN controller as it provides some advantages:
•	Supports broad range of virtual and physical OpenFlow switches.
•	Designed to be high performance.
•	Easy to setup with minimal dependencies. 

Step1: Creation of custom Topology
Created a custom topology in mininet such that analysis of load balancer script is possible. We wrote a python script in which we made API calls to addlink(), addHost, addswitch() .
addlink(): Used to add link either to connect switch-switch or to connect switch-host.
addHost():Used for addition of hosts in network topology by stating its IP address.
addswitch() : Used to add new switches in the network Topology.

Step2: Execution of Topology
Create the topology and run the mininet command for execution specifying IP address of the remote controller. In our case we are using Floodlight VM so we have provided IP address as the localhost address 127.0.0.1.Use the command:
sudo mn --custom Topo.py –topo mytopo –controller=remote,ip=127.0.0.1,port=6653 


B.
Flow of Implementation (Procedure):

1.Start the Floodlight Controller and create a custom Topology in mininet with 12 hosts and x switches in mininet .
2.Test the connectivity by using ping command till there is 0 % drop in the packets.
3.Load balancing is to  be done in two hosts h1 (10.0.0.1) and h4 (10.0.0.4)   
4.Before Load Balancing :Check output ports of switches s1 and s2 to which hosts h1 and h2 are connected .Ping h1 to h4 to verify the packet flow through wireshark. 
Use the iperf command at nodes h1 and h4 to estimate the bandwidth utilization.
5.Run the python script for load balancer. 
6.Enter the hosts in which load balancing is to be done (h1 and h4)
7.Create dictionaries like Linkports {}, path{} etc to store information obtained by accessing floodlight REST API.
8.Retrieve device information like IP address, MAC, ports, switched and store these in corresponding dictionaries.
9.Get the information of all connected switches with its links. Extract the source, destination switches and its corresponding port numbers..
10.	Add an edge in the graph between source and destination nodes. Retrieve latency from switchlinks URI and store it in linkLatency dictionary with its key as the path from source to destination. Store all links in switchlinks dictionary with its corresponding switchIDs.
11.Calculate route/path from source to destination .First the graph will give the shortest paths based on minimum hop counts from source to destination and link costs will be calculated for these paths. Store the paths in path dictionary with key as switchIDs.
12.After fetching route information, access the REST API and retrieve transmission rate and add it to the cost variable. This will give link costs for all the links for the switches in the shortest paths.
13.Find the minimum cost path from source to destination and add a flow rule based on these link costs using staticflowpusher URI.
14.After load balancing: Use iperf command and ping from h1 to h4 and observe the increase in bandwith which verifies Load balancer is working.
Estimate the ping time from source to destination and verify that ping time decreases after running load balancer script.
Find latency of switches in shortest paths by accessing RSET API . Retrieve duration in seconds and byte count to calculate switchlatency and find the total path Latency by adding the switch latency with the link latency retrieved from the switchlinks URI .This value will be low for the chosen minimum cost path .








