# Projeto integrador — Rede do Laboratório de Inteligência Artificial

> **Instituição:** Instituto Federal de Educação, Ciência e Tecnologia do Maranhão — Campus Itapecuru-Mirim
> **Curso:** Técnico em Informática
> **Disciplina:** Redes de Computadores
> **Professor:** Thales Levi Azevedo Valente
> **Duração:** 4 aulas de 50 minutos — 200 minutos efetivos dentro do turno de 4 horas
> **Organização:** equipes de 5 estudantes *(adaptado para 5 integrantes)*

---

## 1. Missão da equipe

O IFMA Campus Itapecuru-Mirim pretende implantar um laboratório de Inteligência Artificial com computadores Linux. Sua equipe deverá elaborar uma **proposta preliminar da rede**, dimensionar os principais recursos e construir uma **simulação reduzida no Cisco Packet Tracer**.

A proposta deve garantir que:

* a rede do laboratório seja logicamente separada da rede geral do campus e a comunicação entre elas seja controlada;
* professor e técnico tenham acesso a todas as máquinas do laboratório;
* cada estudante acesse somente a máquina que lhe foi atribuída;
* duas máquinas de IA sejam reservadas ao uso dos professores, mantendo o acesso administrativo do técnico;
* estudantes possam criar, modificar, executar e excluir seus próprios contêineres;
* somente professor e técnico possam instalar ou alterar sistema operacional, drivers e aplicativos no sistema hospedeiro;
* usuários autorizados possam entrar remotamente, inclusive a partir de outro ponto do IFMA, por um acesso controlado;
* nenhuma máquina de IA seja exposta diretamente à Internet.

> **Segurança:** use somente nomes, endereços e configurações fictícios. Não pesquise, registre ou divulgue senhas, endereços, topologia ou configurações reais da rede do IFMA.

### Entrega obrigatória

A equipe entregará apenas:

1. este arquivo Markdown preenchido e renomeado como `equipe-N-projeto-rede-ia.md`;
2. a simulação `equipe-N-rede-ia.pkt`;
3. três imagens: topologia completa, um acesso permitido e um acesso bloqueado.

Não é necessário produzir slides ou outro relatório.

### O que o Packet Tracer comprova — e o que ele não comprova

O Packet Tracer permite representar endereçamento, VLANs, roteamento, ACLs e o caminho das mensagens. No modo *Simulation*, também permite observar eventos e o percurso de uma PDU (CISCO NETWORKING ACADEMY, [s. d.]).
Entretanto, a simulação **não comprova**:

* identidade real do usuário;
* autenticação por chave SSH;
* permissões de arquivos no Linux;
* isolamento de contêineres *rootless*;
* uso de GPU;
* segurança completa da futura implantação.

Na simulação, um endereço IP representará um usuário autenticado. Na rede real, esse controle também dependerá de contas individuais, chaves SSH, permissões Linux, gateway de acesso e regras no sistema hospedeiro.

---

## 2. Organização das quatro aulas

| Aula | Tempo | Trabalho principal | Resultado ao final |
| --- | --- | --- | --- |
| 1 | 50 min | compreender o problema, dividir papéis e definir requisitos e permissões | seções 3 a 5 preenchidas |
| 2 | 50 min | dimensionar equipamentos, portas e endereços; revisar a proposta | seções 6 a 8 preenchidas e validadas |
| 3 | 50 min | abrir a topologia-base e configurar a simulação guiada no Packet Tracer | VLANs, gateways e ACLs configurados |
| 4 | 50 min | executar seis testes, registrar evidências e concluir | `.md` e `.pkt` revisados e entregues |

---

## 3. Identificação e papéis

| Campo | Preenchimento da equipe |
| --- | --- |
| Nome ou número da equipe | **Alpha morreu** |
| Turma | **Informática-Subsequente** |
| Data | **22/07/2026** |

| Integrante | Papel principal | Responsabilidade |
| --- | --- | --- |
| **Gabriel Rodrigues** | Analista | Define os requisitos e regras da rede |
| **Pedro Eduardo** | Documentador | Preenche a documentação do projeto |
| **Joaquim Gustavo** | Documentador / Testador | Ajuda na documentação e faz os testes |
| **Ryan Cardoso** | Orçamento | Faz o orçamento dos equipamentos |
| **Marcus Antônio** | Projetista-simulador | Faz a simulação no Packet Tracer |

Os papéis organizam o trabalho, mas todas as decisões devem ser conferidas pelos integrantes.

---

## 4. Problema, objetivo e fronteira

### 4.1 Problema

Explique o problema em **até quatro linhas**.

> **Resposta da equipe:**
> O campus precisa de um laboratório de IA seguro e isolado da rede principal. É necessário controlar quem acessa o local e garantir que os alunos não interfiram nas máquinas uns dos outros.

### 4.2 Objetivo

Complete em **até três linhas**:

> Nossa equipe propõe uma rede para **o novo laboratório de IA**, capaz de **isolar o tráfego usando VLANs e ACLs**, mantendo **a segurança dos dados e da rede geral do IFMA.**

### 4.3 Fronteira do trabalho

| Dentro desta atividade | Fora desta atividade de 4 aulas |
| --- | --- |
| requisitos e usuários | compra ou instalação de equipamentos reais |
| estimativa de máquinas e portas | orçamento, marcas e preços |
| topologia reduzida | levantamento da rede institucional real |
| IPv4 privado com redes `/24` | VLSM, IPv6, DHCP, DNS e NAT |
| VLAN, roteamento e ACL guiados | VPN ou WireGuard configurado |
| testes com `ping` ou Simple PDU | instalação real de Linux, SSH, Podman ou drivers |

---

## 5. Requisitos e permissões

| ID | Requisito obrigatório | Estratégia mínima indicada | Conferência da equipe |
| --- | --- | --- | --- |
| R01 | Separar logicamente a rede do laboratório e controlar sua comunicação com o campus. | sub-redes/VLANs e fronteira de roteamento | [X] manter [ ] ajustar: |
| R02 | Impedir que um usuário comum do campus inicie acesso às máquinas do laboratório. | ACL na entrada do laboratório | [X] manter [ ] ajustar: |
| R03 | Dar a professor e técnico acesso a todas as máquinas do laboratório. | contas administrativas e regras de rede autorizadas | [X] manter [ ] ajustar: |
| R04 | Fazer cada estudante acessar somente a máquina atribuída. | ACL representativa + conta e chave SSH individuais | [X] manter [ ] ajustar: |
| R05 | Reservar duas máquinas para uso dos professores, com acesso técnico para manutenção. | zona própria e ausência de contas estudantis | [X] manter [ ] ajustar: |
| R06 | Permitir acesso remoto autenticado sem expor cada máquina diretamente à Internet. | VPN/gateway institucional e SSH | [X] manter [ ] ajustar: |
| R07 | Permitir aos estudantes controlar seus próprios contêineres sem administrar o hospedeiro. | Podman *rootless* por usuário | [X] manter [ ] ajustar: |
| R08 | Restringir sistema, drivers, usuários e aplicativos do hospedeiro a professor e técnico. | contas sem `sudo` para estudantes | [X] manter [ ] ajustar: |
| R09 | Reservar portas e endereços para crescimento. | margem de expansão e plano IPv4 organizado | [X] manter [ ] ajustar: |

### 5.1 Matriz de acesso

| Origem | Destino | Serviço ou ação | Resultado desejado | Teste relacionado |
| --- | --- | --- | --- | --- |
| Professor | qualquer máquina do laboratório | administração e uso | permitir | T1 |
| Técnico | qualquer máquina e equipamento | administração e manutenção | permitir | T2 |
| Alunos dos campus | máquinas atribuídas aos alunos | SSH e uso do ambiente | permitir | T3 |
| Usuário comum do campus | nenhuma máquina do laboratório | - | bloquear | T4 |

Explique em **até três linhas** por que permitir o acesso do professor e bloquear um usuário comum não é uma contradição.

> **Resposta da equipe:**
> O professor precisa gerenciar as aulas e o ambiente. O usuário comum não tem motivos para acessar um laboratório restrito, seguindo o princípio do menor privilégio.

---

## 6. Responsabilidades no Linux e nos contêineres

| Ação | Aluno | Professor | Técnico |
| --- | --- | --- | --- |
| entrar local ou remotamente na máquina atribuída | sim | sim | sim |
| acessar máquinas atribuídas a outros estudantes para administração | não | sim | sim |
| acessar as duas máquinas exclusivas | não | sim | sim |
| baixar e construir imagens de contêiner | sim, em sua conta | sim | sim |
| criar, modificar, iniciar, parar e excluir seus contêineres | sim | sim | sim |
| criar e excluir seus volumes e redes internas de contêiner | sim | sim | sim |
| instalar bibliotecas e aplicações dentro de seus contêineres | sim | sim | sim |
| usar `sudo` no sistema hospedeiro | não | sim | sim |
| instalar ou remover Podman no hospedeiro | não | sim | sim |
| instalar drivers de GPU, alterar kernel ou sistema operacional | não | sim | sim |
| criar usuários e modificar a rede do hospedeiro | não | sim | sim |

### Explique a diferença

Em **até quatro linhas**, explique por que instalar uma biblioteca dentro do contêiner não é a mesma coisa que instalar um aplicativo ou driver no sistema hospedeiro.

> **Resposta da equipe:**
> O contêiner é isolado e afeta apenas o usuário que o criou. Instalar algo no sistema hospedeiro afeta a máquina inteira e exige permissão de administrador (`root`), o que os alunos não têm.

---

## 7. Dimensionamento do laboratório real

### 7.1 Usuários e equipamentos

| Item | Quantidade proposta | Como a equipe estimou? |
| --- | --- | --- |
| máquinas de IA para estudantes | **20** | **Tamanho padrão de uma turma.** |
| máquinas de IA exclusivas para professores | `2` | requisito obrigatório |
| computador de administração técnica | `1` | administração local |
| gateway/servidor de acesso remoto | `1 serviço` | pode ser uma VM ou serviço institucional existente |
| pontos de acesso sem fio | `0`, salvo justificativa | Wi-Fi não faz parte da simulação obrigatória |
| outros dispositivos cabeados | **0** | **-** |

### 7.2 Portas de switch

| Cálculo | Resultado |
| --- | --- |
| total de dispositivos cabeados | **24** |
| uplinks necessários | **1** |
| reserva de 20% | **5** |
| total de portas necessárias | **30** |
| solução escolhida: switch(es) de 24 ou 48 portas | **1 switch de 48 portas** |

### Justificativa

Explique a escolha dos switches em **até três linhas**.

> **Resposta da equipe:**
> Um switch de 48 portas atende aos 30 acessos necessários e ainda deixa espaço de sobra para futuras expansões sem precisar comprar outro equipamento.

---

## 8. Zonas e plano IPv4

### 8.1 Plano fixo da simulação

| Zona simulada | Rede/máscara | Gateway | Finalidade |
| --- | --- | --- | --- |
| acesso pelo campus | `10.10.10.0/24` | `10.10.10.1` | representar usuários que chegam ao roteador do laboratório |
| Equipe A | `172.20.21.0/24` | `172.20.21.1` | máquina atribuída ao Aluno A |
| Equipe B | `172.20.22.0/24` | `172.20.22.1` | máquina atribuída ao Aluno B |
| professores | `172.20.30.0/24` | `172.20.30.1` | duas máquinas exclusivas |

### 8.2 Proposta para o laboratório real

| Zona proposta | Quantidade de hosts prevista | Rede privada sugerida | O que deve acessar? |
| --- | --- | --- | --- |
| máquinas de estudantes | **20** | **172.16.10.0/24** | **Suas máquinas e internet.** |
| máquinas de professores | `2` | **172.16.20.0/24** | **Todas as máquinas de IA.** |
| gerenciamento | **5** | **172.16.99.0/24** | **Equipamentos de rede.** |
| acesso remoto autenticado | **50** | **172.16.200.0/24** | **Acesso SSH ao laboratório.** |

> **Decisão da equipe:** explique em até três linhas por que as zonas não devem formar uma única rede sem controle.
> Separar em zonas permite bloquear acessos indevidos com regras de segurança (ACLs) e impede que problemas em uma rede afetem as outras.

### Checkpoint do professor

* [X] Requisitos revisados.
* [X] Quantidades e portas coerentes.
* [X] Não foram usados dados reais do campus.
* [X] Plano IPv4 sem redes repetidas.
* [X] Matriz de acesso aprovada.
* [X] Professor autorizou o início da simulação.

---

## 9. Arquitetura proposta e 10. Simulação guiada

*(Pule para a seção 11, pois as etapas 9 e 10 são imagens/diagramas fornecidos no template base)*

---

## 11. Testes e evidências

| Teste | Origem | Destino | Esperado | Observado | Coerente? |
| --- | --- | --- | --- | --- | --- |
| T1 | `PC-PROF` | `IA-A` | permitir | **Reply from 172.20.21.10** | **sim** |
| T2 | `PC-TEC` | `IA-PROF-1` | permitir | **Reply from 172.20.30.11** | **sim** |
| T3 | `PC-ALUNO-A` | `IA-A` | permitir | **Reply from 172.20.21.10** | **sim** |
| T4 | `PC-ALUNO-A` | `IA-B` | bloquear | **Destination host unreachable** | **sim** |
| T5 | `PC-ALUNO-A` | `IA-PROF-1` | bloquear | **Destination host unreachable** | **sim** |
| T6 | `PC-CAMPUS` | `IA-A` | bloquear | **Destination host unreachable** | **sim** |

### Registro das três imagens

| Evidência | Arquivo ou imagem inserida | O que demonstra? |
| --- | --- | --- |
| E1 — topologia completa | **[topologia_lab.png]** | organização dos dispositivos e zonas |
| E2 — acesso permitido | **[teste_t1_permitido.png]** | **PC-PROF alcança com sucesso a IA-A.** |
| E3 — acesso bloqueado | **[teste_t4_bloqueado.png]** | **PC-ALUNO-A é bloqueado ao tentar pingar IA-B.** |

### Diagnóstico, se necessário

| Teste com problema | Causa encontrada | Correção realizada | Novo resultado |
| --- | --- | --- | --- |
| **não houve** | **-** | **-** | **-** |

---

## 12. Síntese da proposta

Escreva **de 80 a 120 palavras**.

> **Síntese da equipe:**
> Nosso projeto cria uma rede segura para o laboratório de IA usando um switch de 48 portas. A rede foi dividida em zonas para isolar o campus, os professores, os alunos e o gerenciamento. Usando VLANs e ACLs, garantimos que alunos acessem apenas suas próprias máquinas e rodem contêineres sem privilégios de administrador. Professores e técnicos têm controle total. O Packet Tracer validou o bloqueio e a permissão de acessos, mas na prática será necessário configurar chaves SSH e permissões no Linux para garantir a segurança real.

### Duas evoluções futuras

1. **VPN para acesso remoto seguro** de fora do campus.
2. **Servidor de autenticação** para centralizar o login dos alunos.

---

## 13. Registro da participação

| Integrante | Principal contribuição | O que conferiu no trabalho de outro integrante? |
| --- | --- | --- |
| **Gabriel Rodrigues** | **Definiu as regras de rede e acessos (Analista).** | **Conferiu os testes e a simulação.** |
| **Pedro Eduardo** | **Preencheu toda a documentação do projeto.** | **Conferiu o orçamento feito pelo Ryan.** |
| **Joaquim Gustavo** | **Ajudou na documentação e executou os testes de ping.** | **Conferiu se a simulação do Marcus estava certa.** |
| **Ryan Cardoso** | **Pesquisou e montou o orçamento dos equipamentos.** | **Conferiu a quantidade de portas no projeto.** |
| **Marcus Antônio** | **Montou a rede e configurou tudo no Packet Tracer.** | **Conferiu a documentação preenchida pelo Pedro.** |

| Ferramenta | Pergunta ou tarefa solicitada | O que foi aproveitado? | Como a equipe verificou? |
| --- | --- | --- | --- |
| **Google Gemini** | **"Atue como um Engenheiro de Redes Cisco..." para scripts de configuração.** | **Usamos os comandos do roteador/switch e as explicações teóricas.** | **Testamos os comandos no Packet Tracer e tudo funcionou.** |

---

## 14. Checklist de entrega

* [X] O Markdown está preenchido de forma sucinta.
* [X] A estimativa do laboratório real inclui duas máquinas dos professores.
* [X] O cálculo de portas inclui uplink e reserva.
* [X] O arquivo `.pkt` abre sem erro.
* [X] Dispositivos possuem nomes legíveis.
* [X] Não há conflito de endereços IP.
* [X] Professor e técnico alcançam as máquinas previstas.
* [X] O Aluno A alcança `IA-A`, mas não `IA-B` nem as máquinas dos professores.
* [X] O usuário comum do campus não inicia acesso ao laboratório.
* [X] Os seis testes estão registrados.
* [X] Há exatamente três evidências visuais.
* [X] A síntese possui entre 80 e 120 palavras.
* [X] Nenhum dado real ou credencial do IFMA foi incluído.
