# Vagrant sandbox for centos-7.1, docker-engine, aws cli, Kubectl
#################################################################
# Provisioning script

$script = <<SCRIPT
echo Provisioning ...

# Ref https://docs.docker.com/engine/installation/linux/centos/

yum -y update 
yum -y install linux-headers-$(uname -r) build-essential 
yum -y install gcc kernel-devel dkms make bzip2 perl
sudo mkdir /media/VBoxGuestAdditions
sudo mount -o loop,ro VBoxGuestAdditions.iso /media/VBoxGuestAdditions
sudo sh /media/VBoxGuestAdditions/VBoxLinuxAdditions.run

tee /etc/yum.repos.d/docker.repo <<-EOF
	[dockerrepo]
	name=Docker Repository
	baseurl=https://yum.dockerproject.org/repo/main/centos/7/
	enabled=1
	gpgcheck=1
	gpgkey=https://yum.dockerproject.org/gpg
EOF
# ("<<-EOF" suppresses leading tabs when outputting,
# ref http://www.tldp.org/LDP/abs/html/here-docs.html)

yum install -y docker-engine


# Add the vagrant user to the docker group
# (note this is better than using 'usermod'; see
# https://www.redhat.com/archives/rhl-list/2004-September/msg02595.html)
gpasswd docker -a vagrant

systemctl enable docker.service
systemctl start docker

# Install Kubectl 
curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
chmod +x ./kubectl
mv ./kubectl /usr/local/bin/kubectl
wget https://storage.googleapis.com/kubernetes-release/release/v0.17.0/bin/darwin/amd64/kubectl -O /usr/local/bin/kubectl
chmod +x /usr/local/bin/kubectl
kubectl version

SCRIPT

############################

Vagrant.configure("2") do |config|
  # Using bento/centos-7.1 as the official centos/7 box didn't support
  # synced folders (http://stackoverflow.com/questions/34176041/
  # vagrant-with-virtualbox-on-windows10-rsync-could-not-be-found-on-your-path)
  boxname = 'bento/centos-7.1'
  config.vm.box = boxname

  config.vm.define :toolbox do |toolbox|
    toolbox.vm.provider "virtualbox" do |v|
    	v.customize ['modifyvm',:id, '--memory','2048']
    	v.customize ['modifyvm',:id, '--cpus','2']
    end
    toolbox.vm.hostname = 'centos7'
    #toolbox.vm.network :forwarded_port, guest: 80, host: 4570
    toolbox.vm.provision 'shell', inline: $script,
    	run: "always"
  end
end
