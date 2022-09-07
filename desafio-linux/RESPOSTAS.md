1 - Entrar em modo single user na tela do GRUB, adicionando o parâmetro rd.break ao final da linha 'linux'

montar o sistema de arquivos em leitura/escrita com o comando:

mount -o remount,rw /sysroot

criar uma gaiola chroot 

chroot /sysroot

adicionar o usuário vagrant ao grupo wheel

usermod -aG wheel vagrant

passwd vagrant

reboot

2 - 
groupadd -g 2222 getup
useradd -u 1111 -g getup -G bin getup

visudo

adicionar linha
getup	ALL=(ALL)	NOPASSWD:ALL

3 - 

Criei as chaves na máquina local 

ssh-keygen -t ecdsa

ssh-copy-id -i ~/.ssh/id_ecdsa.pub vagrant@192.168.0.45

ssh vagrant@192.168.0.45


