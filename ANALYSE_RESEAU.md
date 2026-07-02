# Analyse de la topologie reseau

## Synthese

Les fichiers exportes montrent une topologie d'entreprise coherente en trois couches avec segmentation VLAN, routage inter-VLAN sur le coeur, sortie vers un firewall ASA puis un routeur de bordure, et des regles de securite ICMP controlees. Les points principaux sont conformes a la description fournie.

## 1. Architecture L2

- Le coeur active le routage L3 et porte les VLANs 10, 20, 30, 40, 50 et 99 via des interfaces SVI. Voir [Core-Switch-L3_startup-config.txt](Core-Switch-L3_startup-config.txt#L16), [Core-Switch-L3_startup-config.txt](Core-Switch-L3_startup-config.txt#L109), [Core-Switch-L3_startup-config.txt](Core-Switch-L3_startup-config.txt#L113), [Core-Switch-L3_startup-config.txt](Core-Switch-L3_startup-config.txt#L118), [Core-Switch-L3_startup-config.txt](Core-Switch-L3_startup-config.txt#L123), [Core-Switch-L3_startup-config.txt](Core-Switch-L3_startup-config.txt#L128), [Core-Switch-L3_startup-config.txt](Core-Switch-L3_startup-config.txt#L133).
- Les trunks 802.1Q sont bien presentes entre le coeur et les autres couches. Voir [Core-Switch-L3_startup-config.txt](Core-Switch-L3_startup-config.txt#L50), [Core-Switch-L3_startup-config.txt](Core-Switch-L3_startup-config.txt#L97), [Core-Switch-L3_startup-config.txt](Core-Switch-L3_startup-config.txt#L102), [Distribution-Switch-1_running-config.txt](Distribution-Switch-1_running-config.txt#L18), [Distribution-Switch-1_running-config.txt](Distribution-Switch-1_running-config.txt#L70), [Distribution-Switch-2_running-config.txt](Distribution-Switch-2_running-config.txt#L18), [Distribution-Switch-2_running-config.txt](Distribution-Switch-2_running-config.txt#L70), [Server-Switch_running-config.txt](Server-Switch_running-config.txt#L31).
- Les switches d'acces sont bien segmentees par departement: VLAN 20 pour le premier switch, VLAN 30 pour le second, VLAN 40 pour le troisieme et VLAN 50 pour le quatrieme. Voir [Acces-Switch-1_running-config.txt](Acces-Switch-1_running-config.txt#L18), [Acces-Switch-2_running-config.txt](Acces-Switch-2_running-config.txt#L18), [Acces-Switch-3_running-config.txt](Acces-Switch-3_running-config.txt#L18), [Acces-Switch-4_running-config.txt](Acces-Switch-4_running-config.txt#L18).
- Le switch serveur isole la ferme de serveurs dans le VLAN 10 et remonte en trunk vers le coeur. Voir [Server-Switch_running-config.txt](Server-Switch_running-config.txt#L18), [Server-Switch_running-config.txt](Server-Switch_running-config.txt#L31).

## 2. Routage et connectivite L3

- Le coeur fait office de passerelle inter-VLAN avec `ip routing` et une route par defaut vers le firewall. Voir [Core-Switch-L3_startup-config.txt](Core-Switch-L3_startup-config.txt#L16) et [Core-Switch-L3_startup-config.txt](Core-Switch-L3_startup-config.txt#L139).
- Le firewall relie le reseau interne `10.0.0.0/8` a l'exterieur `203.0.113.0/30`, avec routes statiques des deux cotes. Voir [Firewall_running-config.txt](Firewall_running-config.txt#L7), [Firewall_running-config.txt](Firewall_running-config.txt#L14), [Firewall_running-config.txt](Firewall_running-config.txt#L28), [Edge-Router_running-config.txt](Edge-Router_running-config.txt#L18), [Edge-Router_running-config.txt](Edge-Router_running-config.txt#L31), [Edge-Router_running-config.txt](Edge-Router_running-config.txt#L39), [Edge-Router_running-config.txt](Edge-Router_running-config.txt#L42).
- La route de retour sur le routeur de bordure vers `10.0.0.0/8` est bien presente, ce qui est indispensable pour la reponse du trafic sortant. Voir [Edge-Router_running-config.txt](Edge-Router_running-config.txt#L42).

## 3. Services d'infrastructure

- Le coeur relaie le DHCP vers l'adresse `10.0.10.20` via `ip helper-address` sur les VLANs utilisateurs et management. Voir [Core-Switch-L3_startup-config.txt](Core-Switch-L3_startup-config.txt#L116), [Core-Switch-L3_startup-config.txt](Core-Switch-L3_startup-config.txt#L121), [Core-Switch-L3_startup-config.txt](Core-Switch-L3_startup-config.txt#L126), [Core-Switch-L3_startup-config.txt](Core-Switch-L3_startup-config.txt#L131), [Core-Switch-L3_startup-config.txt](Core-Switch-L3_startup-config.txt#L136).
- Le coeur envoie ses journaux vers `10.0.10.30`, ce qui confirme la centralisation de supervision. Voir [Core-Switch-L3_startup-config.txt](Core-Switch-L3_startup-config.txt#L144).
- Les fonctions HTTP, DNS et DHCP sont plausibles et coherentes avec la topologie, mais elles ne sont pas visibles dans ces exports de switch/router; elles doivent etre validees dans les configs des serveurs ou dans le fichier Packet Tracer.

## 4. Securite ASA

- L'ASA est bien configure avec deux zones: `inside` a 100 et `outside` a 0. Voir [Firewall_running-config.txt](Firewall_running-config.txt#L7) et [Firewall_running-config.txt](Firewall_running-config.txt#L12).
- L'inspection d'etat est activee pour ICMP, DNS, FTP et TFTP via la policy globale. Voir [Firewall_running-config.txt](Firewall_running-config.txt#L33), [Firewall_running-config.txt](Firewall_running-config.txt#L38), [Firewall_running-config.txt](Firewall_running-config.txt#L42).
- Le filtrage entrant sur l'interface outside n'autorise que les reponses ICMP echo-reply, ce qui bloque les initiations externes vers le reseau interne. Voir [Firewall_running-config.txt](Firewall_running-config.txt#L28).

## Conclusion

La configuration conforte l'architecture decrite: segmentation VLAN, trunks operants, routage inter-VLAN sur le coeur, transit firewall/edge router, DHCP relay et journalisation centralisee, avec une politique ASA restrictive sur les flux entrants. Le seul point qui reste hors de preuve directe dans ces fichiers est le fonctionnement exact des services HTTP/DNS/DHCP sur les serveurs, car ils ne figurent pas dans les exports textes presentes ici.