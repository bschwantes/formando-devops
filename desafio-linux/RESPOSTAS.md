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

3.1 vi /etc/ssh/sshd\_config
PasswordAuthentication no

3.2 Criei as chaves na máquina local 

ssh-keygen -t ecdsa

Copiei a chave pública para a VM
ssh-copy-id -i ~/.ssh/id\_ecdsa.pub vagrant@192.168.0.45

ssh vagrant@192.168.0.45

3.3

Converter o arquivo para formato de Private Key
base64 -d id\_rsa-desafio-linux-devel.gz.b64 > chave.gz
gzip -d chave.gz
chmod 600 chave
vi chave
:e ++ff=unix #mostra os caracteres EOL
Apagar os caracteres EOL
:wq

converter a chave de OpenSSH para RSA
ssh-keygen -p -m pem -f chave

No SSH server, modificar a permissão do authorized\_keys do user devel

chmod 600 ~devel/.ssh/authorized\_keys

4 - 

systemctl status nginx.service

vi /etc/nginx/nginx.conf

adicionar ';' no final da linha 45
trocar as portas listen para 80

vi /usr/lib/systemd/nginx.service
remover -BROKEN do ExecStart

systemctl daemon-reload
systemctl start nginx.service

curl http://127.0.0.1
> Duas palavrinhas pra você: para, béns!

5 - 


6 - 
6.1 O comando já estava funcionando. Nada foi feito.

6.2
curl -I https://httpbin.org/response-headers?hello=world

HTTP/2 200 
date: Wed, 07 Sep 2022 20:15:44 GMT
content-type: application/json
content-length: 89
server: gunicorn/19.9.0
hello: world
access-control-allow-origin: *
access-control-allow-credentials: true

6.3

Criar arquivo de configuração
vi /etc/logrotate.d/nginx

/var/log/nginx/\*.log {
	rotate 8
	weekly
	compress
	missingok
	notifempty
}

7
7.1

