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

✦ Criar massas ``labredes`` na raiz ``/`` e subpastas
```bash
sudo mkdir /labredes
cd /
```

✦  Verificar se a pasta `labredes` foi criada
```bash
ls -la
```

✦  Verificar a existência do diretório /labredes
```bash
 ls -la / #verificar a existencia do diretório /labredes
 cd /labredes 
 ls -la / #verificar a existencia do diretório /labredes/imagens
 sudo mkdir images
 cd images
 sudo mkdir original 
 cd original
 ls -la

# cria diretórios e subdiretórios
 cd /
 sudo mkdir labredes/VM
 sudo mkdir labredes/VM/913
 sudo mkdir labredes/VM/913/<nome> # substitua <nome> pelo seu nome
```

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

#ATENÇÃO: Se não houver os arquivos iso na pasta /labredes/images/original deve-se copiá-los com os comandos:
scp aluno@192.168.101.10:~/Public/iso-images/ubuntu-server-mini.ova /labredes/images/original
```

## Instalação do Virtualbox Extension Pack
```
su redes
sudo apt install virtualbox-ext-pack
```

## Criando um ambiente de rede com 8 máquinas virtuais com um switch

Topologia de Rede virtualizada dentro do VitualBox para fins de execução adequada do projeto
![topologia-proj 2b](https://user-images.githubusercontent.com/103418874/184263254-be12a2ea-4bbb-401d-95db-10fc5710086c.png)



