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

