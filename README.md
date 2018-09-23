LightSpeed Install with Ubuntu
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 011AA62DEDA1F085
sudo wget -O /etc/apt/trusted.gpg.d/lst_repo.gpg http://rpms.litespeedtech.com/debian/lst_repo.gpg
wget -O - http://rpms.litespeedtech.com/debian/enable_lst_debain_repo.sh | bash