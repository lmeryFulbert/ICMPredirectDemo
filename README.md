# ICMPredirectDemo
```bash
vagrant up
```

## Connexions SSH

dans 3 terminal différents
```bash
vagrant ssh client
```bash
vagrant ssh fakerouter
```
```bash
vagrant ssh routeur
```

## Retroconception du réseau

Faire un schéma réseau de l'infrastructure présenté.

## Lancer une capture TCPdump pour observer les ICMP_Redirects

### Sur le client dans une connexion SSH active

```bash
vagrant@client# sudo tcpdump -i eth1 icmp -w capture_icmp_redirect.pcap &
```
le & permet de reprendre la main sur le shell, le tcpdump (la capture de trame) continue de tourner en arrière plan.
Appuyer sur Entrer pour retrouver votre shell.

Faire un ping vers un serveur publique (par exemple 1.1.1.1 ou 8.8.8.8)

Fermer le processus de capture.

Trouver l'identifiant du processus du tcpdump
```bash
vagrant@client#ps aux | grep tcpdump
```
Trouver l'identifiant du processus de tcpdump
```bash
vagrant@client#sudo kill <process_id_de_tcpdump>
```
ou plus simplement

```bash
sudo pkill tcpdump
```
