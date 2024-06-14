# SAE-reseau
Mignan Baptiste - Hun Valentin
### Instruction
Après avoir répati l'adresse en sous-réseau comme présenter sur la fichier gns3, nous avons configurer les interfaces des routeurs, utiliser le protocole RIP et DHCP puis finir par s'occuper du firewall

### Configuration des interfaces des routeurs.

### R1

configure terminal
interface e0/0
ip address 170.50.192.30 255.255.255.224
no shutdown
end

configure terminal
interface e0/1
ip address 170.50.192.65 255.255.255.240
no shutdown
end

configure terminal
interface e0/2
ip address 170.50.192.97 255.255.255.240
no shutdown
end

wr

### R2

configure terminal
interface e0/0
ip address 170.50.192.66 255.255.255.240
no shutdown
end

configure terminal
interface e0/1
ip address 170.50.192.62 255.255.255.224
no shutdown
end

configure terminal
interface e0/2
ip address 170.50.192.82 255.255.255.240
no shutdown
end

wr

### R3

configure terminal
interface e0/0
ip address 170.50.192.98 255.255.255.240
no shutdown
end

configure terminal
interface e0/1
ip address 170.50.192.81 255.255.255.240
no shutdown
end

configure terminal
interface e0/2
ip address 170.50.192.254 255.255.255.128
no shutdown
end

wr


### Protocole RIP

### R1

configure terminal
router rip
version 2
no auto-summary
network 170.50.192.0
network 170.50.192.64
network 170.50.192.96
end

wr

### R2

configure terminal
router rip
version 2
no auto-summary
network 170.50.192.32
network 170.50.192.64
network 170.50.192.80
end

wr

### R3

configure terminal
router rip
version 2
no auto-summary
network 170.50.192.80
network 170.50.192.128
network 170.50.192.96
end

wr

### DHCP

### Serveur (inutile car c'est sur R1)
ip 170.50.192.1/27 170.50.192.30

### R1
configure terminal
service dhcp
ip dhcp pool finances
network 170.50.192.0 255.255.255.224
default-router 170.50.192.30
end

configure terminal
service dhcp
ip dhcp pool R&D
network 170.50.192.32 255.255.255.224
default-router 170.50.192.32
end

configure terminal
service dhcp
ip dhcp pool production
network 170.50.192.128 255.255.255.128
default-router 170.50.192.254
end

configure terminal
service dhcp
ip dhcp pool lana
network 170.50.192.64 255.255.255.240
default-router 170.50.192.65
end

configure terminal
service dhcp
ip dhcp pool lanb
network 170.50.192.96 255.255.255.240
default-router 170.50.192.97
end

configure terminal
service dhcp
ip dhcp pool lanc
network 170.50.192.80 255.255.255.240
default-router 170.50.192.82
end

ip dhcp excluded-address 170.50.192.1 170.50.192.10
ip dhcp excluded-address 170.50.192.33 170.50.192.42
ip dhcp excluded-address 170.50.192.129 170.50.192.38

### R2

configure terminal
interface e0/1
ip helper-address 170.50.192.65
end

### R3

configure terminal
interface e0/2
ip helper-address 170.50.192.97
end

### FireWall