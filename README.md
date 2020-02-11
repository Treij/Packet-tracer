# Packet-tracer
## Konfigurace portu  
`Switch(config)#int <port[fa 0/2]>` // fast ethernet  
`Switch(config)#int <port[po 0/2]>` // port channel  
  
## Konfigurace vlan  
`Switch(config)#vlan <cislo>`  
`Switch(config-vlan)#name <jmeno>`  

## Přiřazení portu do vlany  
`Switch(config)#int <port[fa 0/2]>`   
`Switch(config-if)#switchport mode access`   
`Switch(config-if)#switchport access vlan <cislo>`   

## Nastavení portu jako trunk  
`Switch(config)#int <port[gi 0/1]>`  
`Switch(config-if)#switchport mode trunk`  

## Zobrazit konfiguraci vlan  
`Switch(config)#do show vlan`  

## Nastavit ip vlan  
`Switch(config)#int vlan <cislo>`  
`Switch(config-if)#ip add <adresa> <maska>`  
`Switch(config-if)#no sh` 

## Vypsat mac address table  
`Switch#show mac-address-table`  

## Přidat statický záznam do mac address tabulky  
`Switch(config)#mac-address-table static 0002.1773.80a9 vlan 1 interface fastEthernet 0/1`  

## Zabezpečení portu  
`Switch(config)#interface fastEthernet 0/5`  
`Switch(config-if)#switchport mode access`  
`Switch(config-if)#switchport port-security`  
`Switch(config-if)#switchport port-security mac-address sticky`  
`Switch(config-if)#switchport port-security maximum 1`  
`Switch(config-if)#switchport port-security violation shutdown`  
	připojit koncovou stanici, mac se nalepí na port automaticky  
	vyzkoušet záškodníka, port je down  
	vrátit zpět a nahodit port:  
`Switch(config-if)#shutdown`  
`Switch(config-if)#no shutdown`  
  
## Inter VLAN routing, nezapomenout trunk na switchi co vede k routeru  
  ### nahození portu  
  `Router(config)#int <port[gi 0/0]>`  
  `Router(config)#no shutdown`  
  
  ### vytvořit virtuální rozhraní  
  `Router(config)#int <port.číslo*[gi 0/0.10]>` // *číslo může být jakékoli, doporučeno číslo vlany pro přehlednost  
  `Router(config-subif)#encapsulation dot1Q <číslo vlany[10]>`  
  `Router(config-subif)#ip address <adresa sítě> <maska>`  
  `Router(config-subif)#no shutdown`  
  
## VTP - doména, trunk -> nastavit před tvorbou vlan  
  ### zobrazit konfiguraci  
  `Switch#show vtp status`  
  
  ### změna názvu domény  
  `Switch(config)#vtp domain cisco`  

  ### nastavit mód klient  
  `Switch(config)#vtp mode client`  
  
## Etherchannel  
`Switch(config-if)#channel-group 1 mode on`  
  
## HSRP
### Nastavení hlavní brány na routeru
`Router(config-if)#standby <skupina> ip 192.168.153.254`  
`Router(config-if)#standby <skupina> priority 120`  
`Router(config-if)#standby <skupina> preempt`  
  
### Nastavení ostatních bran
`Router(config-if)#standby <skupina> ip 192.168.153.254`  

### Volitelně nastavit switch
`Switch(config)#int vlan 1`  
`Switch(config-if)#ip add 192.168.154.3 255.255.255.0`  
`Switch(config)#ip default-gateway 192.168.154.254`

### Výpis info
`Router#sh standby`, eventuelně `Router#sh standby brief`

