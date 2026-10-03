# Infraestructura 2: VPN site-to-site entre FortiGate y un equipo de red

Matrícula: **2025-0873**

## Objetivo

Comunicar el usuario con el servidor web a través del enlace VPN y comprobar que esa comunicación solo fluye si el túnel está activo.

En esta infraestructura solo hay un FortiGate. El otro extremo es un equipo de red de otro fabricante. En el laboratorio se usó Ubuntu con strongSwan porque GNS3 no tenía un router Cisco.

## Topología

- ISP: Ubuntu, solo enruta las IP públicas.
- PEER-A: equipo de red del sitio de usuarios. DHCP, NAT y un extremo de la VPN.
- FG-B: FortiGate del sitio del servidor, configurado por GUI.
- PC-User: VPCS, cliente DHCP de la VLAN 10.
- WEB-SRV: servidor HTTPS.

## Direccionamiento

| Equipo | Interfaz | IP |
|---|---|---|
| ISP | ens3 | 202.5.0.1/30 |
| PEER-A | ens3 WAN | 202.5.0.2/30 |
| ISP | ens4 | 8.73.0.1/30 |
| FG-B | port1 WAN | 8.73.0.2/30 |
| PEER-A | ens4 LAN | 10.20.25.1/25 |
| PC-User | e0 | DHCP 10.20.25.20-100/25 |
| FG-B | port2 LAN | 10.8.73.1/28 |
| WEB-SRV | ens3 | 10.8.73.10/28 |

Tráfico interesante: `10.20.25.0/25` hacia `10.8.73.0/28`.

## Qué se configuró

- Red, ruta por defecto y NAT en el FortiGate, por GUI.
- Red, DHCP de la VLAN 10 y NAT en el equipo de usuarios.
- VPN site-to-site entre peers, con clave compartida `FortiLab2026`.
- El NAT no se aplica al tráfico que entra al túnel.
- Servidor web HTTPS.

## Cómo se demuestra el objetivo

1. Con el túnel arriba, el PC hace ping y traceroute a `10.8.73.10`. El primer salto es `10.20.25.1`.
2. Se baja la VPN en el equipo de usuarios y el ping falla.
3. Se levanta la VPN y el ping vuelve a responder.

## Video

Enlace del video de la infraestructura 2:

https://

Reemplaza esa línea por la URL del video cuando lo subas.
