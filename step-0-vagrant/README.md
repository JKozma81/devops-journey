
Felhasználás: virtuális gépek létrehozására szolgál

Előfeltétel: Valamilyen virtualizációt szolgáló alkalmazás legyen telepítve a gépen (docker, virtualbox, stb…)

  

**Vagrant konfigurációs fájlnak 5 fő eleme van:**

-   config.vm.box: Operációs rendszert jelöli
    
-   config.vm.provider: virtualizációt kiszolgáló eszköz, esetünkben virtualbox
    
-   config.vm.network: a host gép kapcsolódásását definiálja a virtuális géphez hálózaton keresztül
    
-   config.vm.synced_folder: a könyvtár neve amiből elérhetőek a tartalmak a virtuális gépből is
    
-   config.vm.provision: telepítendő alkalmazások parancsainak gyűjteménye
    

  

**Konfig létrehozása:**

Előre definiált boxok elérhetőek a [https://app.vagrantup.com/boxes/search](https://app.vagrantup.com/boxes/search) címen

  

-   vagrant init ubuntu/trusty64 parancs létrehozza a konfig fájlt (ez a parancs megtalálható a fenti címen is minden box leírásában)
    

  

**Parancsok:**

-   vagrant up: elindítja a box-ot
    
-   vagrant destroy: eltávolítja a box-ot
    
-   vagrant suspend: felfüggeszti a box futását
    
-   vagrant resume: újra elindítja a box-ot
    
-   vagrant reload: újraindítja a box-ot
    
-   vagrant ssh: belép a box-ban futó gépre
    
-   vagrant provision: újra futtatja a telepítéséket
    

  

**config.vm.provider:**

vb.memory = 2048 - memória megadására szolgál

vb.cpus = 4 - processzor megadására szolgál

  

**config.vm.network:**

A konfigban a guest a virtuális gép portja, a host a fő gépé

NOTE: /etc/hosts-ban megadható egy egyedi “domain név” is pl 192.168.1.0  vgdemo.local

  

**config.vm.synced_folder:**

config.vm.synced_folder ".", "/var/www/html", :nfs => { :mount_options => [“dmode=777”, “fmode=666”]} - biztonságossá teszi az adatok elérését

  

**config.vm.provision:**

config.vm.provision "shell", path: "bootstrap.sh" - egy bootstrap.sh fájlból tölti be a futtatandó parancsok script-jét

  
  
  
  

**Több virtuális gép egy vagrant fájlban:**

servers tömb-ben meghatározzuk a gépek adatait:

	servers=[

		:hostname => “vm1”,

		:box => “os”,

		:ip => “ip”,

		:ssh_port_ “port”

	]

  

majd egy ciklusban végrehajtjuk a konfigurációt:

	servers.each do |machine|

			config.vm.define machine[:hostname] do |node|

			node.vm.box = machine[:box]

			node.vm.hostname = machine[:hostname]

			node.vm.network : private_network, ip: machine[:ip]

			  

			node.vm.network “forwarded_port”, guest: 22, host: machine[_ssh_port], id: “ssh”

			…

		end

	end

  

A többi beállítás ugyanígy bekerül a ciklusba csak a config helyett a node-ra kell hivatkozni

  

**Anyagok:**

-   [Vagrant Crash Course](https://youtu.be/vBreXjkizgo)
    
-   [Complete Vagrant Course 2020](https://youtube.com/playlist?list=PLnFWJCugpwfyInpbM1A435Lrd56jNwZTr)
    
-   [Vagrant Documentation](https://www.vagrantup.com/docs)