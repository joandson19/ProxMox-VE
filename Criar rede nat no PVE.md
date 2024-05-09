## Habilitar rede nat masquerade no proxmox

### Ative os encaminhamento de ip do seu SO
```
# nano /etc/sysctl.conf
net.ipv4.ip_forward=1

# sysctl -p
```

### Certifique-se de estar usando o nftables no seu pve, pois é com ele que será feito o nat.
### Configure o nat masquerade no nftables
```
# nano /etc/nftables.conf

table ip nat {
        chain postrouting {
                type nat hook postrouting priority srcnat; policy accept;
                oif != "lo" ip saddr 10.255.255.0/24 masquerade
        }
}
```

### Reinicie o nftables
```
# service nftables restart
```

### Configure a interface nat que irá ser usada nas vms que receberão ip com nat
```
# nano /etc/network/interface

auto rede_nat
iface rede_nat inet static
	address 10.255.255.1/24
	bridge-ports none
	bridge-stp off
	bridge-fd 0
# Rede Nat Masquerade
```	

### Reinicie o gerenciador de rede
```
# service networking restart
```

## Agora a rede "10.255.255.0/24" está toda masquerade e pode usar em sua vms os ips de 10.255.255.2-10.255.255.254 que estaram navegaveis.
