


# Módulo 6 - Projeto 1

 ## Introdução
O projeto tem como objetivo a construção de uma REST API para uma aplicação desenvolvida anteriormente, cuja finalidade era a gestão de encomendas para fornecer o Continente do Centro Comercial Colombo, tendo em conta o impacto ambiental que a produção e transporte dos seus produtos provocam. Os detalhes da aplicação inicial estão localizados em: https://github.com/ines2357/modulo5_projeto1.
O projeto está desenhado para se fazer um deploy por uma VM da Azure.


## 1. Desenvolvimento da aplicação
- Foi mantida toda a estrutura da aplicação original, sendo acrescentadas novas APIs às diferentes rotas do ficheiro app.py, responsável por executar a aplicação. Estas APIs devolvem os dados em formato JSON. 
- Realizou-se, também, a integração com uma API específica que devolve a lista dos produtos disponíveis, interligando-se ao ficheiro consumidores.py, que gere a seleção dos produtos pelo utilizador. 
- No contexto da máquina virtual (VM), foram adicionados ficheiros de dados em formato CSV através do recurso de partilha de ficheiros do Azure, conforme abordado no tópico 2.5. Consequentemente, efetuaram-se alterações em ficheiros dedicados ao tratamento destes dados, nomeadamente fornecedores.py e transportadoras.py. 

 ### 1.1. Teste das APIs
 Para testar as APIs, utilizou-se a extensão EchoAPI no VSCode, que funcionou como um Client REST.
 - api/produtos
  
 - api/escolha_produtos
 
- api/resumo_impactos

- api/historico


## 2. Clonar Repositório
 O desenvolvimento da aplicação foi feito com recurso a um repositório git, que contém os ficheiros necessários para correr a aplicação python.
 
`git clone git@github.com:tavaresmm/modulo6projeto1.git`

Os ficheiros csv que contém os dados foram adicionados à partilha de ficheiros do armazenamento da Azure (ver tópico 2.5.), pelo que foram apagados do repositório local.

## 3. Criação Armazenamento
- 1º Passo: Criação de um ficheiro de armazenamento 
Na interface gráfica da Azure,  na secção Contas de armazenamento, adicionou-se uma partilha de ficheiros no Browser de armazenamento.

- 2º Passo: Cópia dos ficheiros csv para a partilha de ficheiros

Este passo foi realizado na interface gráfica da Azure. Ao termos o diretório de armazenamento partilhado, foi possível copiar os ficheiros diretamente para a VM. 

- 3º Passo: Criação de diretório de montagem

 `mkdir -p /mnt/<nome>`
 
- 4º Passo: Montagem da partilha de ficheiros (File Share)

```
sudo mount -t cifs //<utilizador>.file.core.windows.net/<fileshare> /mnt/<nome> \
    -o vers=3.0,username=<utilizador>,password=<STORAGE_KEY>,dir_mode=0777,file_mode=0777
```

**Nota:** A STORAGE_KEY é obtida a partir da respetiva conta de armazenamento, na secção "Chaves de Acesso", e corresponde à chave 1.

- 5º Passo: Tornar a montagem permanente

Editar o ficheiro /etc/fstab:

`sudo nano /etc/fstab`

E acrescentar a linha:

`//<utilizador>.file.core.windows.net/<fileshare> /mnt/<nome> cifs vers=3.0,username=<utilizador>,password=<STORAGE_KEY>,dir_mode=0777,file_mode=0777`

- 6º Passo: Atualização da montagem

`sudo mount -a`


## 4.  Execução da aplicação

Os passos seguintes devem ser executados dentro do repositório clonado.
- 1º Passo: Criação do ambiente virtual

`python3 -m venv .venv`

`source .venv/bin/activate`

- 2º Passo: Instalação dos requisitos da aplicação

`pip install -r requirements.txt`

- 3º Passo: Execução da app.py

`python3 app.py`

