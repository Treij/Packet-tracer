##konfigurace portu  
´Switch(config)#int <port[fa 0/2]>´ // fast ethernet
Switch(config)#int <port[po 0/2]> // port channel

// konfigurace vlan
Switch(config)#vlan <cislo>
Switch(config-vlan)#name <jmeno>

// přiřazení portu do vlany
Switch(config)#int <port[fa 0/2]>
Switch(config-if)#switchport mode access 
Switch(config-if)#switchport access vlan <cislo>

// nastavení portu jako trunk
Switch(config)#int <port[gi 0/1]>
Switch(config-if)#switchport mode trunk

// zobrazit konfiguraci vlan
Switch(config)#do show vlan

// nastavit ip vlan
Switch(config)#int vlan <cislo>
Switch(config-if)#ip add <adresa> <maska>
Switch(config-if)#no sh

// vypsat mac address table
Switch#show mac-address-table

// přidat statický záznam do mac address tabulky
Switch(config)#mac-address-table static 0002.1773.80a9 vlan 1 interface fastEthernet 0/1

// zabezpečení portu
Switch(config)#interface fastEthernet 0/5
Switch(config-if)#switchport mode access
Switch(config-if)#switchport port-security
Switch(config-if)#switchport port-security mac-address sticky
Switch(config-if)#switchport port-security maximum 1
Switch(config-if)#switchport port-security violation shutdown
	připojit koncovou stanici, mac se nalepí na port automaticky
	vyzkoušet záškodníka, port je down
	vrátit zpět a nahodit port:
Switch(config-if)#shutdown
Switch(config-if)#no shutdown

// Inter VLAN routing, nezapomenout trunk na switchi co vede k routeru
  // nahození portu
  Router(config)#int <port[gi 0/0]>
  Router(config)#no shutdown

  // vytvořit virtuální rozhraní
  Router(config)#int <port.číslo*[gi 0/0.10]> // *číslo může být jakékoli, doporučeno číslo vlany pro přehlednost
  Router(config-subif)#encapsulation dot1Q <číslo vlany[10]>
  Router(config-subif)#ip address <adresa sítě> <maska>
  Router(config-subif)#no shutdown

// VTP - doména, trunk -> nastavit před tvorbou vlan
  // zobrazit konfiguraci
  Switch#show vtp status

  // změna názvu domény
  Switch(config)#vtp domain cisco

  // nastavit mód klient
  Switch(config)#vtp mode client

// Etherchannel
Switch(config-if)#channel-group 1 mode on
