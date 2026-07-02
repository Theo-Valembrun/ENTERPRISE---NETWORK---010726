# Analyse de la topologie reseau

## Synthese

Cette documentation confirme une architecture d'entreprise en trois couches avec segmentation VLAN, routage inter-VLAN sur le coeur, transit par firewall ASA et routage de bordure vers l'interface externe. L'ensemble des configurations exportees est coherent avec la topologie Packet Tracer.

## Architecture L2

- Le coeur active le routage L3 et porte les VLANs 10, 20, 30, 40, 50 et 99 via des interfaces SVI. Voir [Core-Switch-L3_startup-config.txt](Core-Switch-L3_startup-config.txt#L16), [Core-Switch-L3_startup-config.txt](Core-Switch-L3_startup-config.txt#L109), [Core-Switch-L3_startup-config.txt](Core-Switch-L3_startup-config.txt#L113), [Core-Switch-L3_startup-config.txt](Core-Switch-L3_startup-config.txt#L118), [Core-Switch-L3_startup-config.txt](Core-Switch-L3_startup-config.txt#L123), [Core-Switch-L3_startup-config.txt](Core-Switch-L3_startup-config.txt#L128), [Core-Switch-L3_startup-config.txt](Core-Switch-L3_startup-config.txt#L133).
- Les trunks 802.1Q sont presentes entre le coeur et les couches adjacentes. Voir [Core-Switch-L3_startup-config.txt](Core-Switch-L3_startup-config.txt#L50), [Core-Switch-L3_startup-config.txt](Core-Switch-L3_startup-config.txt#L97), [Core-Switch-L3_startup-config.txt](Core-Switch-L3_startup-config.txt#L102), [Distribution-Switch-1_running-config.txt](Distribution-Switch-1_running-config.txt#L18), [Distribution-Switch-1_running-config.txt](Distribution-Switch-1_running-config.txt#L70), [Distribution-Switch-2_running-config.txt](Distribution-Switch-2_running-config.txt#L18), [Distribution-Switch-2_running-config.txt](Distribution-Switch-2_running-config.txt#L70), [Server-Switch_running-config.txt](Server-Switch_running-config.txt#L31).
- Les switches d'acces sont reparties par departement: VLAN 20 pour le premier switch, VLAN 30 pour le second, VLAN 40 pour le troisieme et VLAN 50 pour le quatrieme. Voir [Acces-Switch-1_running-config.txt](Acces-Switch-1_running-config.txt#L18), [Acces-Switch-2_running-config.txt](Acces-Switch-2_running-config.txt#L18), [Acces-Switch-3_running-config.txt](Acces-Switch-3_running-config.txt#L18), [Acces-Switch-4_running-config.txt](Acces-Switch-4_running-config.txt#L18).
- Le switch serveur isole la ferme de serveurs dans le VLAN 10 et remonte vers le coeur en trunk. Voir [Server-Switch_running-config.txt](Server-Switch_running-config.txt#L18), [Server-Switch_running-config.txt](Server-Switch_running-config.txt#L31).

## Routage et Connectivite L3

- Le coeur joue le role de passerelle inter-VLAN avec `ip routing` et une route par defaut vers le firewall. Voir [Core-Switch-L3_startup-config.txt](Core-Switch-L3_startup-config.txt#L16) et [Core-Switch-L3_startup-config.txt](Core-Switch-L3_startup-config.txt#L139).
- Le firewall relie le reseau interne `10.0.0.0/8` a l'exterieur `203.0.113.0/30`, avec routes statiques des deux cotes. Voir [Firewall_running-config.txt](Firewall_running-config.txt#L7), [Firewall_running-config.txt](Firewall_running-config.txt#L14), [Firewall_running-config.txt](Firewall_running-config.txt#L28), [Edge-Router_running-config.txt](Edge-Router_running-config.txt#L18), [Edge-Router_running-config.txt](Edge-Router_running-config.txt#L31), [Edge-Router_running-config.txt](Edge-Router_running-config.txt#L39), [Edge-Router_running-config.txt](Edge-Router_running-config.txt#L42).
- La route de retour sur le routeur de bordure vers `10.0.0.0/8` est bien presente, ce qui garantit la reponse des flux sortants. Voir [Edge-Router_running-config.txt](Edge-Router_running-config.txt#L42).

## Services d'Infrastructure

- Les switches assurent le relais DHCP vers l'adresse `10.0.10.20` via `ip helper-address` sur les VLANs utilisateurs et management, tandis que le service DHCP est fourni par le serveur de la ferme. Voir [Core-Switch-L3_startup-config.txt](Core-Switch-L3_startup-config.txt#L116), [Core-Switch-L3_startup-config.txt](Core-Switch-L3_startup-config.txt#L121), [Core-Switch-L3_startup-config.txt](Core-Switch-L3_startup-config.txt#L126), [Core-Switch-L3_startup-config.txt](Core-Switch-L3_startup-config.txt#L131), [Core-Switch-L3_startup-config.txt](Core-Switch-L3_startup-config.txt#L136).
- Le coeur envoie ses journaux vers `10.0.10.30`, ce qui confirme la centralisation de la supervision. Voir [Core-Switch-L3_startup-config.txt](Core-Switch-L3_startup-config.txt#L144).
- Les services HTTP, DNS et DHCP sont coherents avec la topologie, mais leur configuration detaillee ne figure pas dans les exports de routeurs et switches. Le service DHCP est a confirmer dans la configuration du serveur, tandis que le relais est assure par les switches. Leur verification complete depend du fichier Packet Tracer ou des configurations serveur.

## Securite ASA

- L'ASA est configure avec deux zones: `inside` a 100 et `outside` a 0. Voir [Firewall_running-config.txt](Firewall_running-config.txt#L7) et [Firewall_running-config.txt](Firewall_running-config.txt#L12).
- L'inspection d'etat est activee pour ICMP, DNS, FTP et TFTP via la policy globale. Voir [Firewall_running-config.txt](Firewall_running-config.txt#L33), [Firewall_running-config.txt](Firewall_running-config.txt#L38), [Firewall_running-config.txt](Firewall_running-config.txt#L42).
- Le filtrage entrant sur l'interface outside n'autorise que les reponses ICMP echo-reply, ce qui bloque les initiations externes vers le reseau interne. Voir [Firewall_running-config.txt](Firewall_running-config.txt#L28).

## Conclusion

La configuration confirme une solution proprement segmente, avec trunking fonctionnel, routage inter-VLAN, transit firewall/edge router, DHCP relay et journalisation centralisee. Le seul element restant a valider hors de ces exports est le detail des services applicatifs sur les serveurs.