# Projeto 2º Bimestre - ISRE
### Grupo 7

## Tabela de definições de nomes e IPS para todas as VMs

![Máscara](https://user-images.githubusercontent.com/103418874/183775042-13aa9933-b94f-4c75-9a28-945a8852b03f.png)

## Inicialmente:

### Realizar os seguintes comandos nos quatro computadores:

✦ Abrir terminal do computador
✦  Logar com o usuário ``redes``
  ⇨ senha: ``admin@Lab92``
```bash
su redes
```

✦ Verificar a existência dos seguintes diretórios

```bash
/labredes/images/original
/labredes/VM/913/<NomeDoAluno>
```

✦ Caso não existam, fazer a verificação de cada subdiretório. Por exemplo:

```bash
/labredes
```

Assim, para cada subdiretório existente, acessá-lo por meio do comando ``cd /<NomeSubdiretório>``. A partir daí, utilizar o comando ``sudo mkdir<NovoSubdiretório>`` para preencher o diretório corrente. Veja o seguinte exemplo, para o caso de ``labredes`` já estar criado, mas ``images`` não:

```bash
cd /labredes
sudo mkdir images
cd /labredes/images
sudo mkdir original
cd /labredes/images/original
```

OBS: A mesma lógica vale para o diretório ``/labredes/VM/913/<NomeDoAluno>``.


✦ Adicionar o usuário ```aluno``` ao grupo ```redes```
```bash
sudo usermod -aG redes aluno
cd /
```

✦ Modificar as permissões de arquivos e pastas
```bash
 sudo chown -R nobody:nogroup /labredes #``chown`` muda o dono da pasta labredes para o usuario nobody e grupo nogroup
 ls -la
 sudo chgrp -R redes /labredes #``chgrp`` altera o proprietário de grupo do diretório ``/labredes`` para o grupo ``redes``
 sudo chmod -R 771 /labredes #``chmod`` altera as permissões do diretório para escrita pelos membros do grupo
 ls -la
 getent group  #lista grupos
```

✦ Verificar se os diretórios existem:
```
mini.iso
ubuntu-20.04.4-desktop-amd64.iso
ubuntu-22.04-live-server-amd64.iso
```

✦ Verificar no resultado a existência dos arquivos .iso
```shell
cd /labredes/images/original
ls -la #lista todos os arquivos

ATENÇÃO: Se não houver os arquivos iso na pasta /labredes/images/original deve-se copiá-los com os comandos:
scp aluno@192.168.101.10:~/Public/iso-images/ubuntu-server-mini.ova /labredes/images/original
```

## Instalação do Virtualbox Extension Pack
```
su redes
sudo apt install virtualbox-ext-pack
```

## Criando um ambiente de rede com 8 máquinas virtuais com um switch

❖ Topologia de Rede virtualizada dentro do VitualBox para fins de execução adequada do projeto
![topologia-proj 2b](https://user-images.githubusercontent.com/103418874/184263254-be12a2ea-4bbb-401d-95db-10fc5710086c.png)


### 1.Importar VMs no VirtualBox
* O arquivo .OVA é um formato de exportação de VM utilizado pelo VirtualBox
* Esse arquivo será usado para criar as VMs
✦ Para importar:
* No VirtualBox => arquivo => importar appliance...
* Diretório e nome do arquivo a ser importado: /labredes/images/original/ubuntu-server-mini.ova
* Diretório onde será salva a VM: /labredes/VM/913/nome

#### Criando VMs a partir da importação do arquivo OVA
❖ Figura 2: Ilustra a criação das VMs já definidas:
![Captura de Tela (61)](https://user-images.githubusercontent.com/103418874/184268694-5f3cdeca-5f4b-4fbe-955e-f9f402a27fb6.png)

### 2.Configurando as NICs das VMs
* Nas VMs, acessar as configurações de rede de cada VM e selecionar o modo ``rede interna``
* Definir o nome da rede ``labredes`` como nome da rede virtual
* Observação: utilizar o mesmo nome nas duas VMs

### 3.Fazendo login nas VMs
✦ Usuário da VM: ``administrador``
✦ Senha da VM: ``adminifal``

❖ Figura 3: Telas das VMs em execução:
![Redes (1)](https://user-images.githubusercontent.com/103418874/184270662-080edc60-414b-4e6f-9a72-89ab99c600c2.png)

### 4.Configuração estática na interface de rede de endereço IP 
* O Ubuntu utiliza um arquivo YAML, que se encontra na pasta ``/etc/netplan/``, para configurar as interfaces de rede
* Digite os comandos:
```shell
ifconfig -a
ls -la /etc/netplan
cat /etc/netplan/01-netcfg.yaml
```

✦ Verifique o nome correto do arquivo no seu servidor. No exemplo a seguir, o nome do arquivo é ***01-netcfg.yaml***

#### Na VM-Lab01
✦ Instale as ferramentas de rede
```bash
$ sudo apt install net-tools -y
```
✦ Edite o arquivo  ***01-netcfg.yaml*** 
```bash
$ sudo nano /etc/netplan/01-netcfg.yaml
```
✦ Adicione as linhas para a configuração estática do IP para configurar o IP para ``192.168.13.100/28``. 
```
network:
    ethernets:
        enp0s3:                           # nome da interface que está sendo configurada. Verifique com o comando 'ifconfig -a'
            addresses: [192.168.13.100/28]    # IP e Máscara do Host.
            gateway4: 192.168.13.100         # IP do Gateway
            dhcp4: false                  # dhcp4 false -> cliente DHCP está desabilitado, logo o utilizará o IP do campo 'addresses'
    version: 2
```
✦ Após salvar o arquivo é necessário aplicar as configurações, com o **netplan apply**. Depois veja a configuração das interfaces com ****ifconfig -a***
```bash
$ sudo netplan apply
$ ifconfig -a
```
#### Na VM-Lab02
✦ Instale as ferramentas de rede
```bash
$ sudo apt install net-tools -y
```
✦ Edite o arquivo  ***01-netcfg.yaml*** 
```bash
$ sudo nano /etc/netplan/01-netcfg.yaml
```
✦ Adicione as linhas para a configuração estática do IP para configurar o IP para ``192.168.13.101``.
```
network:
    ethernets:
        enp0s3:                           # nome da interface que está sendo configurada. Verifique com o comando 'ifconfig -a'
            addresses: [192.168.13.101/28]    # IP e Máscara do Host.
            gateway4: 192.168.13.97          # IP do Gateway
            dhcp4: false                  # dhcp4 false -> cliente DHCP está desabilitado, logo o utilizará o IP do campo 'addresses'
    version: 2
```
✦ Após salvar o arquivo (ctrl + x) é necessário aplicar as configurações, com o **netplan apply**. Depois veja a configuração das interfaces com ****ifconfig -a***
```bash
$ sudo netplan apply
$ ifconfig -a
```

❖ Figura 4: Configuração do arquivo netplan para rede interna
![Captura de Tela (70)](https://user-images.githubusercontent.com/103418874/184286319-6c4de91b-035a-4c7b-9efe-ad2d49f65098.png)

### 5.Configuração da rede interna do VirtualBox
❖ Figura 5: Ilustra as configurações para a importação das VMs | Configuração das NICs como modo ``rede interna``
![Captura de Tela (64)](https://user-images.githubusercontent.com/103418874/184277653-d9419900-b696-43a2-90d8-06735c983643.png)

###  <sub>Teste a conectividade entre as VMs com o comando ``ping``</sub>

   * Ping da VM1 para VM2
```shell
ping 192.168.13.101    
```
   * Ping da VM2 para VM1
```shell
ping 192.168.13.100   
```
❖ Figura 6: Ilustra o ping das VMs
![Captura de Tela (63)](https://user-images.githubusercontent.com/103418874/184275949-e1e3360c-0794-4ebb-8bd6-62dc1f6d23f5.png)

### <sub>Para finalizar o comando</sub>
```shell
ctrl + c
```

## Conectando as máquinas virtuais através do modo ``bridge``

✦ Desligam-se as VMs
✦ Conectar os cabos no switch
✦ Configurar as VMs para o modo bridge nos adaptadores de rede das VMs
✦ Gerar novos endereços MAC para todas as VMs automaticamente

❖ Figura 7: Configurando a máquina para o modo bridge
![Captura de Tela (68)](https://user-images.githubusercontent.com/103418874/184285711-6fbd2b42-63ca-4e01-900d-5b665d5e5689.png)

###  <sub>Teste a conectividade entre as VMs com o comando ``ping``</sub>

.............
   * PINGS
.............

### Configurando o acesso remoto via ssh nos servidores dos PCs

✦ Dando nomes aos servidores ``hostname``
```bash
sudo hostnamectl set-hostname <hostname>
```
✦ Esse comando será executado em cada VM

#### Instalando o servidor SSH

✦ Antes de iniciar: 
*Altere o Adaptador1 para NAT
*Edite o arquivo netplan conforme a imagem abaixo:

...............
...............

✦ Certifique-se de que a VM está acessando a internet
```bash
sudo apt update   
sudo apt upgrade -y  
```

✦ Instale o SSH
```bash
systemctl status ssh
sudo apt-get install openssh-server
systemctl status ssh
```

✦ Analisar o status das portas do sistema
```bash
netstat -an | grep LISTEN. 
```

❖ Figura 8: Status do ssh
![Captura de Tela (71)](https://user-images.githubusercontent.com/103418874/184290155-e4a4bd44-0de8-4703-b26e-7417de26b4d4.png)

✦ Firewall
* Permitir conexões remotas seguras 
```bash
sudo ufw status
sudo ufw allow ssh. 
sudo ufw status
```

#### <sub>Para ativar o firewall: </sub>
```
sudo ufw enable
```

❖ Figura 9: status do firewall
![Captura de Tela (72)](https://user-images.githubusercontent.com/103418874/184290611-b8804ced-109e-4c84-90c3-0e221252f1c7.png)


