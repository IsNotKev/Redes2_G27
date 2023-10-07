# Proyecto #1 - Grupo #27

---

### Integrantes
- Kevin Steve Martinez Lemus - 202004816
- Javier Alejandro Gutierrez de León - 202004765 

---
### Topología


### Direcciones de VLAN
|                  |VLAN |
|------------------|-----|
| VLAN19           |19   |
| VLAN29           |29   |
| VLAN39           |39   |

### Direcciones de red para los sectores de la VLAN Corporativa
|           | IP              |
|-----------|-----------------|
| Verde      | 192.168.19.0/24 |
| Anaranjado | 192.168.29.0/24 |
| Entre MSW | 192.168.39.0/24 |

#### Configurando VTP
```
enable
conf t
vtp mode [client | server]
vtp domain g27
end
```

### Configurando VLANs
```
enable
conf t
vlan 19
name VLAN19
vlan 29
name VLAN29
vlan 39
name VLAN39
exit
exit
```

### Habilitando modo trunkal
El modo trunkal es capaz de transmitir datos de múltiples VLANs a través de un solo enlace físico, manteniendo la separación de las VLANs mediante el uso de etiquetas.
```
enable
conf t
interface fastEthernet0/1
switchport trunk encapsulation dot1q
switchport mode trunk
end
```

### Habilitando modo acceso
El modo acceso se utiliza para conectar dispositivos finales, como computadoras o impresoras a una única VLAN específica sin necesidad de etiquetas adicionales.
```
enable
conf t
interface range f0/11-12
switchport mode access
switchport access vlan 19
exit
exit
```

## Interfaz de VLAN

```
conf t
interface vlan 39
ip address 192.168.39.1 255.255.255.0
no shutdown
end
```

### Configurando HSRP 

```
interface vlan 39
ip address 192.168.39.1 255.255.255.0
standby 39 ip 192.168.39.254
standby 39 priority 110
standby 39 preempt
```

```
interface vlan 39
ip address 192.168.39.2 255.255.255.0
standby 39 ip 192.168.39.254
standby 39 priority 100
standby 39 preempt
```


### Configuraciones LACP
```
ena
conf t
interface Port-Channel 2
switchport trunk encapsulation dot1q
switchport mode trunk
no shutdown
exit
int range f0/1-3
channel-protocol lacp
channel-group 2 mode active
switchport trunk encapsulation dot1q
switchport mode trunk
end
wr
```

#### Protocolo OSPF
```
en
conf t
ip routing
router ospf 10
network 192.168.19.0 0.0.0.255 area 10
network 192.168.29.0 0.0.0.255 area 10
network 192.168.39.0 0.0.0.255 area 10
exit
exit
wr
```

### Prompts hechos a Chat GPT

Configurar HSRP
![Prompt a ChatGPT](./img/prompt1.png)
![Prompt a ChatGPT](./img/prompt2.png)

Configuracion trunk
![Prompt a ChatGPT](./img/prompt3.png)
![Prompt a ChatGPT](./img/prompt3.2.png)