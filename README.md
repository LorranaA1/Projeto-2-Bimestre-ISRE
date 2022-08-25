# Projeto 2º Bimestre - ISRE
### Grupo 7

* Içami Costa Silva
* Marcos Felype Silva
* Maria Lorrana Alves Barbosa
* Vitor Daniel de Oliveira Magalhães

### ``DEFINIÇÕES``

## Tabela de definições de nomes e IPS para todas as VMs

![Máscara](https://user-images.githubusercontent.com/103418874/183775042-13aa9933-b94f-4c75-9a28-945a8852b03f.png)

## Usuários
![Redes (13)](https://user-images.githubusercontent.com/103418874/186592475-159ae721-0d9c-4c9e-8d0c-40e76bb03b93.png)
![Redes (14)](https://user-images.githubusercontent.com/103418874/186592921-94cbe0ea-e2c3-4a4f-9fff-29e467dd6cd5.png)

## Configuração de hardware utilizada

✦ VMs

![IMG_20220825_082651](https://user-images.githubusercontent.com/103426684/186652640-4159cf89-e0f6-4216-ba4a-fb2bc3c81712.jpg)

✦ Tela inicial

![ConfiguracaoHardware](https://user-images.githubusercontent.com/103418874/186572061-9afea2aa-4cf0-4d81-bc68-9e92326e1db9.png)

✦ Rede Física

![IMG-20220812-WA0054](https://user-images.githubusercontent.com/103426684/186651107-78c43c5e-caf3-42fc-8ff4-776f2f96cb2e.jpg)

## Softwares

* VirtualBox
* SSH server
* Net-tools
* Ubuntu server

### ``CONFIGURAÇÃO/INSTALAÇÃO``

## Inicialmente:

### Realizar os seguintes comandos nos quatro computadores:

* ✦ Abrir terminal do computador
* ✦ Logar com o usuário ``redes``
* ⇨ senha: ``admin@Lab92``
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

⚠ ATENÇÃO: Se não houver os arquivos iso na pasta /labredes/images/original deve-se copiá-los com os comandos:
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
* ✦ Para importar:
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
* ⇨ Usuário da VM: ``administrador``
* ⇨ Senha da VM: ``adminifal``

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

#### ⚠ ATENÇÃO - Fazer para todas as máquinas

### 5.Configuração da rede interna do VirtualBox
❖ Figura 5: Ilustra as configurações para a importação das VMs | Configuração das NICs como modo ``rede interna``
![Captura de Tela (64)](https://user-images.githubusercontent.com/103418874/184277653-d9419900-b696-43a2-90d8-06735c983643.png)

###  <sub>Teste a conectividade entre as VMs com o comando ``ping``</sub>

Exemplo:
   * Ping da VM1-PC2 para VM2-PC2
```shell
ping 192.168.13.101    
```
   * Ping da VM2-PC2 para VM1-PC2
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

* ✦ Desligam-se as VMs
* ✦ Conectar os cabos no switch
* ✦ Configurar as VMs para o modo bridge nos adaptadores de rede das VMs
* ✦ Gerar novos endereços MAC para todas as VMs automaticamente

❖ Figura 7: Configurando a máquina para o modo bridge
![Captura de Tela (68)](https://user-images.githubusercontent.com/103418874/184285711-6fbd2b42-63ca-4e01-900d-5b665d5e5689.png)

###  <sub>Teste a conectividade entre as VMs com o comando ``ping``</sub>

Exemplo: 
* Ping da VM1-PC3 para VM2-PC1
```bash
ping 192.168.13.99
```
* Ping da VM2-PC1 para VM2-PC3
```bash
ping 192.168.13.103
```
* Realizar o ping de todos para todos

``PS: Os testes de conectividade entre as VMs no modo bridge estão na última seção (TESTES/VALIDAÇÃO)``

#### OBSERVAÇÃO: o ping foi realizado de todos para todos

### Configurando o acesso remoto via ssh nos servidores dos PCs

✦ Dando nomes aos servidores ``hostname``
```bash
sudo hostnamectl set-hostname <hostname>
```
✦ Esse comando será executado em cada VM

#### Instalando o servidor SSH

✦ ⚠ ATENÇÃO - Antes de iniciar: 
* 1º. Nas configurações de rede: altere o Adaptador1 para NAT
* 2º. No arquivo netplan: Edite o arquivo seguindo as instruções abaixo
```bash
✦ Comente o addresses
✦ Comente o gateway4
✦ Coloque o dhcp4 como true
```

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

### Refazendo a topologia de rede
* No arquivo do netplan, retorne as configurações de rede para o modo bridge em cada VM no VirtualBox e ative o endereçamento IP estático.

✦ Acessando uma máquina virtual
```bash
ssh <user>@<ipServidorRemoto> 
```

### Acesso Remoto SSH com (Host Only) no Virtual Box

* ✦ Login da VM ubuntu server
* ⇨ Usuário: ``administrador``
* ⇨ senha: ``adminifal``

#### Criando uma interface no computador visando a comunicação entre o PC e a VM e configurando o servidor DHCP no adaptador VBoxNet0.:

* No VirtualBox, clique em Arquivo e depois Host Network Manager

* NA ABA ADAPTADOR:

  ❖ Figura 10: VirtualBox
    
    <img width="795" alt="VirtualBox" src="https://user-images.githubusercontent.com/103418874/186584618-03b6fc4d-0a83-4c22-a7ea-3274f085099e.png">

* NA ABA SERVIDOR DHCP:

  ❖ Figura 11: VirtualBox DHCP
  
    <img width="785" alt="VirtualBox-dhcp" src="https://user-images.githubusercontent.com/103418874/186584630-4ac78d86-0930-487d-b704-d8caf974ed71.png">

* Verifique a configuração das interfaces usando o ``Terminal do computador``

  ❖ Figura 12: Interfaces
  
    ![Redes (3)](https://user-images.githubusercontent.com/103418874/186584871-5a292f54-d90a-44bc-b66c-c2244e99b5e2.png)

#### Adicionando um adaptador (HostOnly) em uma VM

``Para acessar a uma VM via rede pelo terminal do PC é preciso criar um novo adapatador de rede à VM``

 ❖ Figura 13: Criação do adaptador de rede

   ![Redes (7)](https://user-images.githubusercontent.com/103418874/186586240-d1b7a6f2-3cd3-47fc-baad-1a43354f847c.png)
    
#### Ativando as configurações da Interface na VM para o servidor DHCP

* Verificando a existência da interface “enp0s8”

  ❖ Figura 14: Verificando...
  
    ![Redes (9)](https://user-images.githubusercontent.com/103418874/186587289-d495bd6f-8f1e-4b58-8325-eb2138e3a3d8.png)
* Configurando as interfaces no netplan e ativando o DHCP para o Adaptador 2 (enp0s8):

  ❖ Figura 15: Configurando...
  
    ![Redes (10)](https://user-images.githubusercontent.com/103418874/186588563-242b1936-379e-4e4a-abbc-68e3a19274b1.png)
* Aplicando as configurações:
```shell
sudo netplan apply
```
* Averiguando se pegou IP na nova interface de rede:

  ❖ Figura 16: Averiguando...
    ![Redes (11)](https://user-images.githubusercontent.com/103418874/186589201-424495b3-143b-4726-aef2-987d00c1145c.png)

### Acessando uma VM remotamente:

* Exemplo: $ ssh ``<user>``@``<ipServidorRemoto>``
* Fazendo o login 
   * de: terminal-pc
   * para: 192.168.13.100

```shell
ssh <user>@<ipServidorRemoto>
```
``PS: Os testes de acesso remoto estão na última seção (TESTES/VALIDAÇÃO)``

## Por fim, o mapeamento IP/Nomes no arquivo /etc/hosts de cada VM

❖ Figura 17: Mapeamento
![Redes (12)](https://user-images.githubusercontent.com/103418874/186591521-7890be13-76e2-4cb7-b81a-32a2b81c7f1e.png)

### ``TESTES/VALIDAÇÃO``

#### Testes de conectividade entre as VMs no modo bridge

❖ Modo bridge

![Captura de tela de 2022-08-25 09-40-28](https://user-images.githubusercontent.com/103418874/186724999-41fd0c20-52d6-4902-a550-0f91270d9f6c.png)
![Captura de tela de 2022-08-25 09-37-56](https://user-images.githubusercontent.com/103418874/186725003-7bc38649-dd1f-4f7c-94e1-c6e15ede444c.png)
![Captura de tela de 2022-08-25 09-41-51](https://user-images.githubusercontent.com/103418874/186725004-382403b8-f963-4ab7-b98c-7d1e61ce9fe7.png)
![Captura de tela de 2022-08-25 09-44-00](https://user-images.githubusercontent.com/103418874/186725006-efe8fc23-eddd-4e53-8457-aa2989f41edd.png)

#### Testes de SSH sem conexão física

❖ SSH

![Captura de tela de 2022-08-25 08-52-16](https://user-images.githubusercontent.com/103418874/186724717-4d984a55-3b2e-4a65-8692-858aad70551f.png)
![Captura de tela de 2022-08-25 08-58-17](https://user-images.githubusercontent.com/103418874/186724725-85e7c3c6-2457-4de5-8799-657c6e916d20.png)
![Captura de tela de 2022-08-25 08-59-03](https://user-images.githubusercontent.com/103418874/186724726-8343cee2-7d61-4393-aa59-accc2f17b9e2.png)
![Captura de tela de 2022-08-25 08-55-57](https://user-images.githubusercontent.com/103418874/186724728-05c0215f-503b-43c3-a4ce-3bd3d167079c.png)
![Captura de tela de 2022-08-25 08-59-47](https://user-images.githubusercontent.com/103418874/186724729-2b290903-6b20-4a5f-b3b0-5974628757d5.png)
![Captura de tela de 2022-08-25 08-57-23](https://user-images.githubusercontent.com/103418874/186724732-4f625342-e92c-404b-8bd1-367c26d20dc7.png)
![Captura de tela de 2022-08-25 08-48-10](https://user-images.githubusercontent.com/103418874/186724734-f0c8b495-44f5-49ee-8acd-abe720ae7522.png)
![Captura de tela de 2022-08-25 08-50-00](https://user-images.githubusercontent.com/103418874/186724737-f4c2af1c-bf92-4bfe-9867-fa7353ec0d5c.png)
![Captura de tela de 2022-08-25 08-51-44](https://user-images.githubusercontent.com/103418874/186724742-5c35b7da-b8b0-456a-955d-b0746c4a6f78.png)

#### Testes de acesso a uma VM via rede pelo terminal do PC

❖ Host-Only
