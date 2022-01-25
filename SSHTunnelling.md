

Reference : https://www.youtube.com/watch?v=N8f5zv9UUMI  

Local Port Forwarding:  
  - Access remote resources that I cant access directly ( resource is in some internal network )
  - e.g Internal Remote Database
  - Create a local server that listens on 8888 ( tunnel to public ssh server )
  
        local       <--- tunnel:22 ---->   public ssh server  ----> internal n/w machine
     port - (8888)                            4.1.2.3:22             
      10.0.0.4                                192.168.1.2            192.168.1.33:8080      
 
  > ssh -L 8888:192.168.1.33  4.1.3.2  
  
  TCP Packet on the tunnel , Public SSH server receives and   
  forwards to 192.168.1.33:8080  
 
  
   1. On my Router configure Firewall port forwarding.  
      All traffic on port 22 and my public ip , forward to raspberry  
   
   > ssh -L 8888:raspberrypi:80 pi@remotehost  
  
  
  
  Remote Port Forwarding:  
  - Want someone to access my resources they dont have access to.  
  - e.g Local Server 
