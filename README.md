# estudo-oracle-cloud
Resumo de estudo para certificação


__Regions__ - é uma área geográfica que tem 1 ou mais domínios de disponibilidades (AD)

__Domínios de disponibilidades AD__ - significa que tem 1 ou mais data centers localizados em uma região, mas conectados uns aos outros por uma rede (exteran, pois não compartilhar recursos, energia, refrigeração...). Cada AD são isolados um de outro, tolerante a falhas.

__Domínios de falha AF__ - são um agrupamento de hardware e infa dentro de um AD, como se fosse data centers lógicos, um AD pode possuir vários domínios de falhas, sendo assim, se um equipamento falhar, apenas aquele AF será impactado.

__AIM__ - Controle de acesso refinado é um médoto que controla os acessos usando funções e privilégios.

__PRINCIPALS__ - é uma entidade do IAM que tem permissão para interagir com recursos do OCI. Um 'principal' pode ser uma pessoa ou um recurso externo(app)

__Princípios de Instancia (Instance Principle)__ - permite que instancia de app façam chamadas para API.

__Os COMPARTIMENTOS__ podem ser armazendados as políticas de acesso que os grupos tem. Os compartimentos são entidades lógicas. Pode-se manter um conjunto de recursos relacionados em compartimentos específicos.Os COMPARTIMENTOS que criar são globais e está disponíveis em todas as regiões em que tiver acesso. 

__Os RECURSOS__ são objetos do cloud, esses recursos são identificados atraves de um id único chamado Oracle Cloud ID ou OCID

__Permissionamento__ = AuthZ, sempre começa tudo sem permissão. E tem que liberar aos grupo (não tem como dar permissão direto ao usuário, apenas ao grupo)
__Autorização__ = AuthN

__Compartilhamento__ = O compartilhamento é basicamente uma maneira de isolar logicamente seus recursos na nuvem, dando acesso somente as pessoas autorizadas.
Cada compartimento pode ter um compartimento filho(chield) e pode haver até 6 níveis de aninhamentos.

__Policy__ = Ao criar uma Policy, pode-se aplicar aos compartilhamento criados. As policy criadas por padrão todos os acessos são negados, você deve apenas liberar o acesso que precisa.

### Networking
Loadbalance, o OCI prové um loadbalance de camada 3, 4 e 7(http).
Path request balance: pode-se fazer o balance pelo path de entrada.ex: oracle.com.

Netword Load Balance:
* Faz health check por ping
* Default 5 tuples de hash: ip de origem, ip de destino, numero do protocolo (22), numero da porta e porta de destino. Com esses dados é gerado o hash. 
mas pode setar 3 ou 2 tuples para gerar o hash.


### OCI Compute
os tipos de servidores são.
* __Virtual Machine (VM)__ -> São compartilhadas, Multi Tenant. Tem agentes de configuração e monitoramento já prontos pela Oracle.
* __Bare Metal (BM)__ -> Servidor físico exclusivo, único inquilino. Custo mais alto devido a quantidade de OCPU, pois um bare Metal possui muitos.
	Casos de uso deste tipo: Workloads que não podem ser virtualizados(HPC), ou quando vc mesmo precisa gerenciar essa virtualização.
* __Dedicated Host (DVH)__ -> Servidor físico com VM com os recursos e gerenciamentos da Oracle, não compartilhada. 

A primeira dependencia que um compute(server) tem é a rede da nuvem (VCN)

A Oracle sempre entrega o OCPU cheio, cada OCPU tem 2 UCPU.

#### Tipos de custo e disponibilidade e provisionamento da VM
* __On-Demand__: Custo fixo, performance total e durabilidade total do recurso.
* __Capacity Reservation__: Recursos pré alocado com preços mais baixo que não estão em uso mas caso necessário vc irá precisar ter aquele recurso disponível (exemplo de uso: DR Desaster Recovery), performance total e disponibilidade total.
* __Instancia Premptiva__: Tem a preformance total do recurso com custo de 50% menos. Mas a durabilidade do recurso não é garantida, pode ser destruída pela nuvem a qualquer momento. (Muito usado para ambiente Dev, sandbox...)
* __Instancia Burstable__: Quando em determinados momentos se precisar que de uma demanda muito maior, você provisiona mais recursos e diz quando vai usar no modo normal, aí quando se precisa do recurso máximo a Oracle tenta disponibilizar, mas ela não garante o recurso máximo pois nesse modo, tem diversos clientes usando o mesmo OCPU e disputando as UCPU.

### OCI Storage Service
* __NVMe__ -> Alta performance. Non-persistence, mas sobrevive a reboot.
* __Block Bolume__ -> Boa performance, durável e persistence.
* __File Storage__ -> Pode ser adicionando em vários pontos de montagem. Suporta o protocolo NFS v3. Poítica 0 thrust(Por default tudo é bloqueado).
* __Object Storage__ -> Usado pra backup, tem as camadas quente e fria. Quanto mais fria, mas demora para obter aquele arquivo e mais barato é.

#### Load Balancer (Cai na prova)
* __Round-robin__ -> Distribui uma solicitação pra cada backend.
* __Ip hash__ -> Vincula o IP a um servidor, então toda solicitação de um cliente cai no mesmo servidor.
* __Least connection__ -> Envia para o servidor que tem menos conexões

-> O loadbalance de camada 7 atua como network de proxy
-> O loadbalance de camada 3 é non-proxy, vai aparecer o ip pro servidor.

### Autonomous Database
É a parte da infraestrutura dedicada ao banco de dados e com machine learning integado ao banco de dados para ajudar na administração do Database.
(CAI NA PROVA) Tipos de Deployments:
* __Shared__: Ele da o banco como serviço, então a oracle cuida da infraestrutura
* __Dedicated__: A máquina do exadata é dedicada pra você e não compartilha recurso do hardware com outros clientes, e a oracle NÃO gerencia pra você.


os deploymentents podem ser feito pelo tipo de ATP (Autonomous transaction Processing), json(Para banco de dados no-sql), Apex(para app low code) e para data WareHouse - ADW(big data).

(CAI NA PROVA) Comparações entre 
ADW(Autonomous Data Warehouse) |||| ATP(Autonomous Transaction Processing)
	Columnar Format		||	Row Format
	Creates Data Summaries	||	Creates Indexes
     Memory Speeds Jonis, Aggs	||	Memory for Caching to Avoid IO


#### Serviços pré definidos:
* __ATP__(Autonomous Transaction Processing)(5 qtd): High, Medium, low (do high até low fica em fila as transações), TPURGENT(Transações com prioridade máxima), TP(típicas, normalmente usadas em aplicações)
* __ADW__(Autonomous Data Warehouse)(3 qtd): High, Medium, low.

### Datacenter Oracle
__Database System__:
* Capacidade de até 24 CPU, 320GB Ram e 40TB de armazenamento

### OCI Security Services
__Responsabilidades da Oracle__ é parte em compliance com os orgãos de segurança e a segurança física dos servidores.
__Responsabilidades do cliente__: Criar security list, criação de políticas de acesso do usuário.

#### WAF Web Application Firewall
* __Access Control__: por exemplo controle de IP
* __Bot Management__: Bot que faz várias avaliação tentando verificar se a requisição é segura e não é um bot
* __Waf__: Possui diversas regras prontas, e pode ser criado regras novas de acordo com a necessidade.

#### Bastion
Permite acesso publico seguro a recursos de destino que está numa rede privada

# OCI Develop

### Resource Manager
Ele interpleta e executa o terraform(código nativo), fornece a infraestrutura como código, você diz o que precisa (ex: 1 máquina com 1tb de hd, memória x) e o terraform interpleta e gera um código e executa(faz o provisionamento). Com esse código, vc pode executar em qualquer ambiente da oracle cloud.
O anssyble configura os ambientes já provisionado pelo terraform.

O arquivo onde declara a infraestrutura á ser provisionada é o *.tf


No resource manager, há um compartimento de teste e o compartimento de produção.

terminologias:
*stack: é o código dentro da OCI;
*job: é a execução do stack;
