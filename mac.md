#MacAdderss

##Network
Rack4-5(5)      58  NO
I350:           
6C:92:BF:2E:A8:6C      192.168.100.92     
6C:92:BF:2E:A8:6D     
82599 10G:          
6C:92:BF:27:CD:24        192.168.101.192   
6C:92:BF:27:CD:09       192.168.102.92     
BMC         
SHARED          
6C:92:BF:2E:AB:6E           
DEDICATED           
6C:92:BF:2E:AB:6F           
            
Rack4-6(6)      59  NO  
I350:           
6C:92:BF:2E:2D:04           
6C:92:BF:2E:2D:05           
82599 10G:          
6C:92:BF:27:7A:A1           
6C:92:BF:27:CC:FF           
BMC         
SHARED          
6C:92:BF:2E:2D:06           
DEDICATED           
6C:92:BF:2E:2D:07           
            
Rack4-7(7)      60  NO
I350:           
6C:92:BF:2E:2B:C4           
6C:92:BF:2E:2B:C5           
82599 10G:          
6C:92:BF:27:CC:B2           
6C:92:BF:27:CC:F3           
BMC         
SHARED          
6C:92:BF:2E:2B:C6           
DEDICATED           
6C:92:BF:2E:2B:C7           

Rack5-5(55) 67
I350:   
6C:92:BF:2E:2F:C0   
6C:92:BF:2E:2F:C1   
82599 10G:  
6C:92:BF:27:CD:6B   
6C:92:BF:27:CC:F7   
BMC 
SHARED  
6C:92:BF:2E:2F:C2   
DEDICATED   
6C:92:BF:2E:2F:C3   
    
    
Rack5-6(56) 68
I350:   
6C:92:BF:2E:BA:30   
6C:92:BF:2E:BA:31   
82599 10G:  
6C:92:BF:27:CD:6C   
6C:92:BF:27:CD:64   
BMC 
SHARED  
6C:92:BF:2E:BA:32   
DEDICATED   
6C:92:BF:2E:BA:33   

##机器Mac地址记录
        101                     102 
158     6C:92:BF:27:CD:24       6C:92:BF:27:CD:09    
159     6C:92:BF:27:7A:A1       6C:92:BF:27:CC:FF 
160     6C:92:BF:27:CC:B2       6C:92:BF:27:CC:F3 
167     6C:92:BF:27:CD:6B       6C:92:BF:27:CC:F7
168     6C:92:BF:27:CD:6C       6C:92:BF:27:CD:64
    
##vlan的划分：

```
Ruijie>enable 
Ruijie#configure terminal
Ruijie(config)#vlan 10     ------>创建VLAN 10
Ruijie(config-vlan)#vlan 20     ------>  创建VLAN 20
Ruijie(config-vlan)#vlan 30       ------>创建VLAN 30
Ruijie(config-vlan)#exit
Ruijie(config)#interface range GigabitEthernet 0/2-4       ------>配置该端口Gi 0/2-4 都为trunk 口
Ruijie(config-if-range)#switchport mode trunk
Ruijie(config-if-range)#exit
Ruijie(config)#interface vlan 10      ------>创建SVI 10
Ruijie(config-if)#ip address 192.168.10.1 255.255.255.0      ------>配置vlan 10的网关地址
Ruijie(config-if)#interface vlan 20      ------> 创建SVI 20
Ruijie(config-if)#ip address 192.168.20.1 255.255.255.0       ------> 配置vlan 20的网关地址
Ruijie(config-if)#interface vlan 30        ------>创建SVI 30
Ruijie(config-if)#ip address 192.168.30.1 255.255.255.0      ------> 配置vlan 20的网关地址
Ruijie(config-if)#end      ------>退出到特权模式
Ruijie#write             ------>确认配置正确，保存配置

```