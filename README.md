# Ansible Project

Dieses Projekt umfasst die Erstellung von zwei MaaS-Instanzen unter Verwendung eines persönlichen Schlüsselpaars, wobei "druettimann" als Lehrer ausgewählt wird. Durch die Implementierung von Ansible in der Windows Subsystem for Linux (WSL) auf dem Notebook erfolgt die Konfiguration für die Verwaltung der MaaS-Instanzen.

Die Automatisierung erfolgt über ein Ansible-Playbook, das die vollständige Installation der Ref-Card-03 durchführt. Dabei werden notwendige Tools für die Java-Jar-Dateierstellung auf dem Webserver installiert, eine Datenbank implementiert und für den Betrieb der jokesdb konfiguriert.

Die Spring Boot-Applikation und die Datenbank werden nach einem Neustart automatisch mit 'systemd' neu gestartet.

## Requirements

- WSL mit Ubuntu
- Zugriff auf ein MaaS

## Getting started

### Installieren von Ansible:
```bash
sudo apt update
sudo apt install ansible
```

### Check Ansible Version:
```bash
ansible --version
```
Die Version muss 2.10.8 oder höher sein.

### Generieren von SSH P/K Schlüssel:
```bash
ssh-keygen
```
Alles mit Enter bestätigen.

### Auslesen des SSH Public Schlüssels:
```bash
cat .ssh/id_rsa.pub
```
Dieser Schlüssel kann nun bei github "https://github.com/settings/keys" (New SSH Key) eingetragen werden.

### Clonen vom Repository auf WSL:
```bash
git clone git@github.com:Sven1222225/maas.git
```
Die Fingerprint frage mit "yes" beantworten.

### Repository mit VS-Code öffnen:
```bash
cd maas/
code .
```
Danach im Project den Public Schlüssel auch noch im **cloud-init.yml** beim Key "ssh_authorized_keys" einfügen.

Kopiere den inhalt vom **Cloud-init.yml** file in die Script section beim MaaS oder lade das File hoch. Dies muss 2 mal gemacht werden da wir einen Webserver und eine Datenbank brauchen.

Wenn die VM gestartet ist können die IP's im **host** File eingetragen werden.

Die IP der Datenbank kann nun auch noch im **springboot-run.service** File beim Key "ExecStart" bei der "-DDB_URL" angepasst werden.

### Das Ansible Inventory Prüfen:
```bash
ansible-inventory --graph
```

### Die Verbindung zu den VM's Prüfen:
```bash
ansible -m ping all
```

### Das Ansible Script prüfen:
```bash
ansible-playbook site.yml --check
```
Einige Tasks werden geskipped, welche von ansible nicht "dry checked" werden können.

### Das Ansible Script ausführen:
```bash
ansible-playbook site.yml
```
Nun kann mit der für den Webserver verwendete IP und dem Port 8080 im Browser eine Verbindung zum MaaS hergestellt werden und die Seite aufgerufen werden.
