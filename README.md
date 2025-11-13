OpenFlow SDN with Ryu Controller Integrated with Snort (Using GNS3 Environment)
Objective
This project demonstrates the integration of OpenFlow SDN using the Ryu controller with Snort, a network intrusion detection system (NIDS), for mitigating Distributed Denial of Service (DDoS) attacks in a network environment. The project uses GNS3 to simulate the network topology and Ubuntu Server to host the Ryu controller and Snort NIDS. The goal is to showcase how SDN can dynamically detect and mitigate various types of network attacks, including ICMP, TCP, and UDP floods, using Ryu’s flow management and Snort’s attack detection capabilities.

<img width="472" height="177" alt="398403341-94413e5f-567d-4225-86aa-38fd95b46d8e" src="https://github.com/user-attachments/assets/25946002-d20a-498e-949f-53c99b0f71b5" />


Skills Learned
Understanding of OpenFlow and SDN architecture.
Configuring Ryu controller and integrating it with OpenFlow-enabled devices.
Installing and configuring Snort as an Intrusion Detection System.
Using GNS3 for creating and managing network simulations.
Implementing dynamic network traffic control for DDoS mitigation.
Working with network mirroring and monitoring traffic for attack detection.
<img width="579" height="345" alt="398402597-b8a8e450-cd9f-4802-a9f0-ab6510ea26da" src="https://github.com/user-attachments/assets/da5ba604-1d73-46e9-9c36-23fa3c438a9c" />


Tools Used
GNS3: For simulating the network topology with Cisco switches, routers, and virtual machines.
Ryu Controller: For managing SDN flow tables and traffic routing.
OpenvSwitch (OVS): For creating virtual switches and mirroring traffic to Snort.
Snort: Network Intrusion Detection System (NIDS) used for detecting and alerting on DDoS attacks.
Ubuntu Server: Hosting the Ryu controller, Snort, and other necessary components.
Hping3: For simulating DDoS attacks in the network.
<img width="596" height="255" alt="image" src="https://github.com/user-attachments/assets/ef2994c6-2bdc-4ae7-ae9b-1dfecbee01c5" />


Steps
1. GNS3 Topology Setup
The GNS3 workspace was populated with the following devices:

•	Four Cisco switches for the core network switches.
•	One Cisco router for routing between networks.
•	Ten host PCs connected to the switches for testing traffic generation.
•	One OpenvSwitch (OVS) integrated into the topology to work with the Ryu controller.
•	Ryu Controller running on an Ubuntu server, responsible for managing traffic and flows.
•	Snort IDS installed on the Ubuntu server to monitor and analyze network traffic.

2. Configuring OpenvSwitch (OVS)
OpenvSwitch (OVS) was installed on the Ubuntu server and configured to act as the central switch in the SDN network. To enable OVS and establish communication with the Ryu controller, the following commands were used:

**Install OVS**
sudo apt install openvswitch-switch

# Start OVS service
sudo service openvswitch-switch start

# Create a bridge and add the interface
sudo ovs-vsctl add-br br0
sudo ovs-vsctl add-port br0 eth0

# Set up traffic mirroring for Snort
ovs-vsctl -- --id=@p get port br0 -- --id=@mirror create mirror name=snort-mirror select-all=true output-port=@p -- -- set Bridge br0 mirrors=@mirror

# Link OVS to the Ryu controller
sudo ovs-vsctl set-controller br0 tcp:192.168.234.236:6633

<img width="602" height="283" alt="image" src="https://github.com/user-attachments/assets/5793e379-26f9-416c-a102-ae72fb021082" />


4. Snort Installation and Configuration
Snort was installed and configured as follows:

# Install Snort
sudo apt-get update
sudo apt-get install snort

# Configure Snort to monitor traffic
sudo snort -A console -c /etc/snort/snort.conf -i eth0
image

5. DDoS Attack Simulation with Hping3
To simulate DDoS attacks, the following commands were used:

ICMP Flood
hping3 --icmp --flood --rand-source <destination_ip>
TCP Flood
hping3 --flood -S --rand-source -p 80 <destination_ip>
UDP Flood
hping3 --udp --flood --rand-source -p 161 <destination_ip>
image

6. Integration of Ryu and Snort for DDoS Mitigation
Network Setup: Ryu controller and Snort are configured to work together on the same server.
Traffic Mirroring: OVS mirrors all network traffic to Snort for inspection.
Attack Detection: Snort analyzes the incoming traffic for known attack patterns (ICMP, TCP, UDP floods).
Alert Generation: Upon detection, Snort generates an alert and sends it to the Ryu controller.
Dynamic Flow Modification: Ryu receives the alert and modifies the network flow table to mitigate the attack by blocking or rerouting malicious packets.
7. Experiment Results
Various types of DDoS attacks were simulated. The integration of Snort and Ryu provided an effective mitigation strategy. Dynamic flow control enabled the SDN network to detect and block malicious traffic in real-time, ensuring the availability and security of the network.

image

Conclusion
This project showcases how OpenFlow SDN and Ryu controller, when integrated with Snort IDS, can effectively mitigate DDoS attacks. By dynamically adjusting traffic flows based on attack detection, the system can prevent network disruptions and maintain service continuity.

For any questions or further assistance for the python code, please contact me - 📧 Email: johnusiabulu@cipherknights.com .

Wireshark analysis in the network during DDOS attacks.
image

Wireshark analysis in the network after DDOS attacks mitigated.
image

OpenVswitch Flow table showing packet dropped.
image
