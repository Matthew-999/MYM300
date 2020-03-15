# Matay Alp / ST17C / 15.03.2020 / Apache Webserver mit Kennwort Authentifizierung

Projekt: Das File hab ich so "Programmiert" das es ein Benutzername und ein Kennwort abgefragt wird, wenn man "localhost:5566" aufruft. 


#### Code
##### HTML
```css
<!doctype html>
<html>
 Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/xenial64" #Hier wird angegeben welcher Typ installiert wird. 
  config.vm.network "forwarded_port", guest:80, host:5566, auto_correct: true #Hier wird das Portforwarding erstellt. Der Port 80 wird somit auf den Port 5566 weitergeleitet. 
  config.vm.synced_folder ".", "/var/www/html"  
config.vm.provider "virtualbox" do |vb|
  vb.memory = "1024"  #In diesem Abschnitt wird der RAM Speicher zugewiesen. 
end
config.vm.provision "shell", inline: <<-SHELL #Hier wird die Provisionierung als Shell 
  # Packages vom lokalen Server holen
  # sudo sed -i -e"1i deb {{config.server}}/apt-mirror/mirror/archive.ubuntu.com/ubuntu xenial main restricted" /etc/apt/sources.list 
  sudo apt-get update #Lädt die Paketlisten aus den Repositorys herunter und "aktualisiert" sie
  sudo apt-get -y install apache2 #Mit diesem Command wird Apache2 heruntergeladen und installiert. 



  sudo apt-get install apache2-utils #Mit apache2-utils werden zusätzliche Features zu apache2 heruntergeladen.
  sudo touch /etc/apache2/.htpasswd #Hier habe ich eine Password datei erstellt. 
  sudo htpasswd -m /etc/apache2/.htpasswd Alp testtesttest testtesttest
  #Mit -m habe ich den Typ ausgewählt und es wurde danach nach einem Benutzernamen und Kennwort gefragt.
  

  sudo nano /etc/apache2/sites-enabled/default.conf #Hier wurde die Konfigurationsdatei von Apache2 aufgerufen
  damit die Kennwortauthentifizierung der gewünschten Seite zugewiesen wird. 
<VirtualHost *:80>
<Location /> #Hier habe ich festgelegt das alle Seiten eine Authentifizierung anfordern sollten. 
	Deny from all
	AuthUserFile /etc/apache2/.htpasswd #In diesem Abschnitt wird der Pfad der Authentifizierungsdatei festgelegt. 
	AuthName "Authentifizierung erforderlich" # Das ist der Name der beim Pop-Up fenster erscheint. 
	AuthType Basic #Der Typ der Authentifizierung
	Satisfy Any
	require valid-user #Benötigt gültige User
    </Location> 
</VirtualHost>

#Fazit: Ich habe mit diesem Auftrag einige neue Erfahrungen gesammelt. 
Das Projekt das ich versuchte, hat wie erwähnt auf der GUI funktioniert, jedoch im Vagrantfile nicht.
Das einzige Problem war, das ich gewünschte Änderungen nicht in eine Datei schreiben konnte. 


SHELL
end
end
```
