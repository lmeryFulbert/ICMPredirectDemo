# TP ICMP Redirect

1.   Clonez ce repository dans votre répertoire de travail.

```bash
git clone https://github.com/lmeryFulbert/ICMPredirectDemo.git
```

2.   Vérifiez que le logiciel Vagrant est bien installé.

```bash
vagrant --version
```

3.   Se placer dans le dossier du dépot

```bash
cd ICMPredirectDemo
```

4.   Lancez l'environnement de machines virtuelles.

```bash
vagrant up
```

Attention: On vous demandera quelle interface il faut associer au bridge, choisissez le bon adaptateur réseau (celui qui est connecté à internet)

## Connexion aux VM via SSH

4.   Dans **3 terminaux différents**, faites :

```bash
vagrant ssh client
```
```bash
vagrant ssh fakerouter
```
```bash
vagrant ssh routeur
```

L'authentification de l'utilisateur nommé ```vagrant``` se fait via une clé SSH. Il n'y a pas besoin de connaître le mot de passe.

## Rétroconception du réseau et questions systèmes basiques

5. Réalisez un schéma réseau de l'infrastructure présentée en utilisant les commandes ```ip a``` et ```ip route```.

6. Faites-le vérifier par votre enseignant.

7. Que concluez-vous sur l'adresse IP de l'interface ```eth0``` de chacune des machines virtuelles ?

8. Vérifiez la présence du dossier ```capture``` dans votre répertoire de travail avec la commande ```ls```.

9.  Vérifiez les droits sur ce dossier. Qui en est le propriétaire ?

10. Connectez-vous en SSH à la VM client et vérifiez le contenu du répertoire personnel ```~```. Quel est son chemin complet ? (variable d'environnement ```$HOME```)

11. Créez un fichier dans le répertoire ```capture``` :
```bash
touch myfile.txt
```

12. Vérifiez sur le système physique que le fichier `myfile.txt` se trouve bien dans le dossier ```capture```.

13. Ouvrez l'interface de VirtualBox. Que constatez-vous ?

14. En lisant la documentation sur les **Shared Folders**, expliquez ce qui se passe et quel est le rôle de Vagrant par rapport à VirtualBox.
   - [Documentation Shared Folders](https://docs.oracle.com/en/virtualization/virtualbox/6.0/user/sharedfolders.html)

## Lancer une capture TCPdump pour observer les ICMP Redirects

### Sur le client dans une connexion SSH active

15. Dans le dossier ```capture```, faites un :

```bash
sudo tcpdump -i eth1 icmp -w capture_icmp_redirect.pcap &
```

Le symbole ```&``` permet de reprendre la main sur le shell ; le tcpdump (la capture de trame) continue de tourner en arrière-plan. Appuyez sur Entrée pour retrouver votre shell.

16.  Faites un ping vers un serveur public (par exemple 1.1.1.1 ou 8.8.8.8).

### Fermer le processus de capture

17. Trouvez l'identifiant du processus tcpdump :

```bash
ps aux | grep tcpdump
```

18. Tuez le processus :
```bash
sudo kill <process_id_de_tcpdump>
```

Ou plus simplement, tuez le processus directement avec son nom :
```bash
sudo pkill tcpdump
```

## Analyse de la capture

19. Sur votre poste physique, ouvrez le fichier généré via le tcpdump (`capture_icmp_redirect.pcap`) avec Wireshark.

20. Quels sont les différents codes ICMP pour : 
   - Une requête d'écho
   - Une réponse d'écho
   - Un message de redirection (ICMP Redirect)

21. Faites une représentation des échanges via un diagramme de séquence (UML) entre la VM Client, la VM Fakerouter et la VM Routeur.
   - [Diagramme de séquence](https://fr.wikipedia.org/wiki/Diagramme_de_s%C3%A9quence)

22. Comment pourrait-on utiliser ce type de message dans une tentative de piratage ?

## Extinction propre des VMs

23. Pour éteindre vos VMs proprement, il suffit de faire :

```bash
vagrant halt
```

N'oubliez surtout pas d'éteindre vos VMs.

24. Si vous souhaitez les détruire définitivement :

```bash
vagrant destroy
```
