# Respostas

## 1. Kernel e Bootloader

Entrar em modo single user na tela do GRUB, adicionando o parâmetro `rd.break` ao final da linha 'linux'

Montar o sistema de arquivos em leitura/escrita com o comando:

```
mount -o remount,rw /sysroot
```

Criar uma gaiola chroot:

```
chroot /sysroot
```

Adicionar o usuário vagrant ao grupo wheel:

```
usermod -aG wheel vagrant
passwd vagrant
reboot
```

## 2. Usuários

### 2.1 Criação de usuários

```
groupadd -g 2222 getup
useradd -u 1111 -g getup -G bin getup
```
Dar permissão de sudo sem senha:

```
visudo
```

E adicionar a linha:

`getup	ALL=(ALL)	NOPASSWD:ALL`

## 3. SSH

### 3.1 Autenticação confiável

```
vi /etc/ssh/sshd_config
PasswordAuthentication no
```

### 3.2 Criação de chaves

Criar as chaves na máquina local:

```
ssh-keygen -t ecdsa
```

Copiar a chave pública para a VM:

```
ssh-copy-id -i ~/.ssh/id_ecdsa.pub vagrant@192.168.0.45
```

Realizar a conexão SSH:

```
ssh vagrant@192.168.0.45
```

### 3.3 Análise de logs e configurações SSH

Converter o arquivo para formato de Chave Primária:

```
base64 -d id_rsa-desafio-linux-devel.gz.b64 > chave.gz
gzip -d chave.gz
chmod 600 chave
vi chave
- :e ++ff=unix #mostra os caracteres EOL
- Apagar os caracteres EOL
- :wq
```

Converter a chave de OpenSSH para RSA:

```
ssh-keygen -p -m pem -f chave
```

No SSH server, modificar a permissão do authorized_keys do usuário `devel`:

```
chmod 600 ~devel/.ssh/authorized_keys
```

## 4. Systemd

```
systemctl status nginx.service #Verifica o erro

vi /etc/nginx/nginx.conf

# Adicionar ';' no final da linha 45
# Trocar as portas listen para 80

vi /usr/lib/systemd/nginx.service
# Remover -BROKEN do ExecStart

systemctl daemon-reload
systemctl start nginx.service
```
Executar o comando:

```
curl http://127.0.0.1
> Duas palavrinhas pra você: para, béns!
```

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
Expandir partição LVM
cfdisk /dev/sdb
Resize [New size: 5G]
Write


