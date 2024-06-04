# instalace samby (pokud nejde tak sudo apt update)
sudo apt install samba

# vytvoreni sdileneho uloziste
sudo mkdir /data

# vytvoreni usera (neni nutne)
sudo adduser samba1

# vytvoreni skupiny sambashare
sudo groupadd sambashare

# pridani uzivatelu do skupiny sambashare
sudo usermod -aG sambashare *user*

# zmena vlastnika uloziste na libovolnou skupinu - tady sambashare
sudo chown :sambashare /data

# zmena prav - pridani prav groupe vlastnici slozku
sudo chmod 770 /data

# uprava konfigurace samby - /etc/samba/smbd.conf
# pridat nakonec souboru 
#
[sambashare]  
    comment = Samba on Ubuntu
    path = /data
    read only = no
    browsable = yes
    writable = yes
    // nazev skupiny ktera ma k ulozisti pristup - zde sambashare
    force group = sambashare
    // stejna skupina jako u force group
    valid users = @sambashare
    create mode = 0660
    directory mode = 0770
#

# vytvoreni hesla pro samba uzivatele
sudo smbpasswd -a *user*

# restartovani samby
sudo systemctl restart smbd
