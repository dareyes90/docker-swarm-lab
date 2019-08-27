#Carpeta compartida
FOLDER_SYNC = "D:/repos"

Vagrant.configure("2") do |config|

  #Imagen base
  config.vm.box = "centos/7"

  #Compartir carpetas especificas
  config.vm.synced_folder ".", "/vagrant", type: "virtualbox"
  config.vm.synced_folder FOLDER_SYNC, "/repos", type: "virtualbox",
  owner: "vagrant",
  mount_options: ["dmode=777,fmode=777"]

  #Exponer el puerto del servicio en el host
  config.vm.network "forwarded_port", guest: 8080, host: 8080

  # Configuracion de las capacidades virtuales
   config.vm.provider "virtualbox" do |v|
      v.memory = "1024"
      v.cpus = 2
  end

  #Configuracion de la VM
  config.vm.provision "shell" do |s|
    s.privileged = false
    s.inline =  <<-SHELL
    #Ajuste de zona horaria
    sudo timedatectl set-timezone America/Bogota
    #Intalacion de docker
    sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
    sudo yum install -y unzip git telnet docker-ce net-tools
    sudo sed -i "s|ExecStart=/usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock|ExecStart=/usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock -H tcp://0.0.0.0:4243 -H unix:///var/run/docker.sock|g" /lib/systemd/system/docker.service
    sudo systemctl enable docker
  SHELL
  end
end
