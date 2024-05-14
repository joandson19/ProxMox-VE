# Instalação e uso do HP Smart Storage Admin CLI (ssacli) no Proxmox PVE

## 1. Adicione o repositório em /etc/apt/sources.list.d/hpe.list:
```
# echo "deb http://downloads.linux.hpe.com/SDR/repo/mcp bullseye/current non-free" > /etc/apt/sources.list.d/hpe.list
```
## 2. Adicione as chaves:
```
# curl -sS https://downloads.linux.hpe.com/SDR/hpPublicKey2048.pub | gpg --dearmor > /etc/apt/trusted.gpg.d/hpPublicKey2048.gpg
# curl -sS https://downloads.linux.hpe.com/SDR/hpPublicKey2048_key1.pub | gpg --dearmor > /etc/apt/trusted.gpg.d/hpPublicKey2048_key1.gpg
# curl -sS https://downloads.linux.hpe.com/SDR/hpePublicKey2048_key1.pub | gpg --dearmor > /etc/apt/trusted.gpg.d/hpePublicKey2048_key1.gpg
```
## 3. Atualize as fontes do apt:
```
# apt update
```
## 4. Instale as ferramentas necessárias:
```
# apt install ssacli
```

# Aqui estão os exemplos de comandos ssacli que você pode usar para diagnosticar e gerenciar seu Controlador de Armazenamento Inteligente HP.

## Mostrar controladores disponíveis

```
ssacli ctrl all show
```

## Mostrar status dos controladores

```
ssacli ctrl all show status
```

## Mostrar informações detalhadas dos controladores

```
ssacli ctrl all show detail
```

## Mostrar configuração dos controladores

```
ssacli ctrl all show config
```

## Rescannear por novos dispositivos Útil após trocar discos nas baías, etc.

```
ssacli rescan
```

## Mostrar todos os discos físicos (ou seu status) (controlador slot 0)

```
ssacli ctrl slot=0 pd all show
ssacli ctrl slot=0 pd all show status
```

## Mostrar informações detalhadas de todos os discos físicos (controlador slot 0)

```
ssacli ctrl slot=0 pd all show detail
```

## Mostrar unidades lógicas (ou seu status) (controlador slot 0, todas ou unidades lógicas específicas)

```
ssacli ctrl slot=0 ld all show
ssacli ctrl slot=0 ld all show status

ssacli ctrl slot=0 ld 1 show
ssacli ctrl slot=0 ld 1 show status
```

## Mostrar informações detalhadas das unidades lógicas (controlador slot 0, todas ou unidades lógicas específicas)

```
ssacli ctrl slot=0 ld all show detail
ssacli ctrl slot=0 ld 1 show detail
```

## Mostrar informações do array (controlador slot 0, array A)

```
ssacli ctrl slot=0 array a show
```

## Mostrar status do array (controlador slot 0, todos os arrays)

```
ssacli ctrl slot=0 array all show status
```

## Criar nova unidade lógica RAID 0 (controlador slot 0, disco na porta 1I:caixa 1:baias 1)

```
ssacli ctrl slot=0 create type=ld drives=1I:1:1 raid=0
```

## Criar nova unidade lógica RAID 1 (controlador slot 0, discos nas portas 1I:caixa 1:baias 1 e 2)

```
ssacli ctrl slot=0 create type=ld drives=1I:1:1,1I:1:2 raid=1
```

## Criar nova unidade lógica RAID 5 (controlador slot 0, discos nas portas 1I:caixa 1:baias 1 a 4)

```
ssacli ctrl slot=0 create type=ld drives=1I:1:1-1I:1:4 raid=5
```

## Excluir unidade lógica (controlador slot 0, unidade lógica 1)

```
ssacli ctrl slot=0 ld 1 delete
```

## Adicionar novos discos físicos à unidade lógica (controlador slot 0, unidade lógica 1, discos nas portas 1I:caixa 1:baias 6 e 7)

```
ssacli ctrl slot=0 ld 2 add drives=1I:1:6,1I:1:7
```

## Adicionar discos sobressalentes (controlador slot 0, unidade lógica 1, array A, discos nas portas 1I:caixa 1:baias 6 e 7)

```
ssacli ctrl slot=0 array a add spares=1I:1:6,1I:1:7
```

## Adicionar discos sobressalentes globais (controlador slot 0, unidade lógica 1, todos os arrays, discos nas portas 1I:caixa 1:baias 6 e 7)

```
ssacli ctrl slot=0 array all add spares=1I:1:6,1I:1:7
```

## Ativar/desativar LED de unidade lógica piscante (controlador slot 0, unidade lógica 1)

```
ssacli ctrl slot=0 ld 1 modify led=on
ssacli ctrl slot=0 ld 1 modify led=off
```

## Ativar/desativar LED de disco físico piscante (controlador slot 0, disco físico na porta 1I:caixa 1:baias 1)

```
ssacli ctrl slot=0 pd 1I:1:1 modify led=on
ssacli ctrl slot=0 pd 1I:1:1 modify led=off
```

## Modificar proporção de leitura e gravação do cache do smart array (controlador slot 0, proporção de cache 80% leitura/20% gravação)

```
ssacli ctrl slot=0 modify cacheratio=80/20
```

## Mostrar status do cache de gravação do disco físico (controlador slot 0)

```
ssacli ctrl slot=0 modify dwc=?
```

## Ativar/desativar cache de gravação do disco físico (controlador slot 0) Importante: Como o cache de gravação do disco físico não é suportado por bateria, você pode perder dados se ocorrer uma falha de energia durante um processo de gravação. Para minimizar essa possibilidade, use uma fonte de alimentação de backup.

```
ssacli ctrl slot=0 modify dwc=enable
ssacli ctrl slot=0 modify dwc=disable
```

## Mostrar status do cache de gravação do smart array quando não há bateria presente (opção de cache de gravação sem bateria, controlador slot 0)

```
ssacli ctrl slot=0 modify nbwc=?
```

## Ativar/desativar cache de gravação do smart array quando não há bateria presente (opção de cache de gravação sem bateria, controlador slot 0)

```
ssacli ctrl slot=0 modify nbwc=enable
ssacli ctrl slot=0 modify nbwc=disable
```

## Ativar/desativar cache do smart array para determinado Volume Lógico (controlador slot 0, unidade lógica 1)

```
ssacli ctrl slot=0 ld 1 modify arrayaccelerator=enable
ssacli ctrl slot=0 ld 1 modify arrayaccelerator=disable
```

## Ativar/desativar SSD Smart Path (controlador slot 0, array A)

```
ssacli ctrl slot=0 array a modify ssdsmartpath=enable
ssacli ctrl slot=0 array a modify ssdsmartpath=disable
```

## Mostrar modo de ativação de sobressalentes

```
ssacli ctrl slot=0 modify spareactivationmode=?
```

## Definir modo de ativação de sobressalentes

```
ssacli ctrl slot=0 modify spareactivationmode=predictive
ssacli ctrl slot=0 modify spareactivationmode=failure
```

## Mostrar prioridade de reconstrução

```
ssacli ctrl slot=0 modify rp=?
```

## Modificar prioridade de reconstrução

```
ssacli ctrl slot=0 modify rp=low
ssacli ctrl slot=0 modify rp=medium
ssacli ctrl slot=0 modify rp=mediumhigh
ssacli ctrl slot=0 modify rp=high
```

## Apagar disco físico (controlador slot 0, disco físico na porta 1I:caixa 1:baias 1)

```
ssacli ctrl slot=0 pd 1I:1:1 modify erase
```
