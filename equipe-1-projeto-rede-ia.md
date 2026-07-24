# Projeto integrador — Rede do Laboratório de Inteligência Artificial

> **Instituição:** Instituto Federal de Educação, Ciência e Tecnologia do Maranhão — Campus Itapecuru-Mirim  
> **Curso:** Técnico em Informática  
> **Disciplina:** Redes de Computadores  
> **Professor:** Thales Levi Azevedo Valente  
> **Duração:** 4 aulas de 50 minutos — 200 minutos efetivos dentro do turno de 4 horas  
> **Organização:** equipe Alpha morreu, com 5 integrantes

---

## 1. Missão da equipe

A equipe Alpha morreu deve propor uma rede segura para um laboratório de Inteligência Artificial, usando apenas nomes, endereços e configurações fictícios. A solução precisa separar o laboratório da rede geral do campus, controlar quem acessa cada máquina e preservar a autonomia dos estudantes dentro de seus próprios contêineres.

A proposta deve garantir que:

- a rede do laboratório seja logicamente separada da rede geral do campus e a comunicação entre elas seja controlada;
- professor e técnico tenham acesso a todas as máquinas do laboratório;
- cada estudante acesse somente a máquina que lhe foi atribuída;
- duas máquinas de IA sejam reservadas ao uso dos professores, mantendo o acesso administrativo do técnico;
- estudantes possam criar, modificar, executar e excluir seus próprios contêineres;
- somente professor e técnico possam instalar ou alterar sistema operacional, drivers e aplicativos no sistema hospedeiro;
- usuários autorizados possam entrar remotamente por acesso controlado;
- nenhuma máquina de IA seja exposta diretamente à Internet.

> **Segurança:** toda a arquitetura abaixo usa endereços fictícios, nomes simulados e regras de acesso organizadas por VLANs e ACLs.

### Entrega obrigatória

A entrega será composta por:

1. este arquivo Markdown preenchido;
2. a simulação Packet Tracer `equipe-1-rede-ia.pkt`;
3. três imagens: topologia completa, acesso permitido e acesso bloqueado.

---

## 2. Organização das quatro aulas

| Aula | Tempo | Trabalho principal | Resultado ao final |
|---|---:|---|---|
| 1 | 50 min | compreender o problema, dividir papéis e definir requisitos e permissões | seções 3 a 5 preenchidas |
| 2 | 50 min | dimensionar equipamentos, portas e endereços; revisar a proposta | seções 6 a 8 preenchidas e validadas |
| 3 | 50 min | abrir a topologia-base e configurar a simulação guiada no Packet Tracer | VLANs, gateways e ACLs configurados |
| 4 | 50 min | executar seis testes, registrar evidências e concluir | `.md` e `.pkt` revisados e entregues |

---

## 3. Identificação e papéis

| Campo | Preenchimento da equipe |
|---|---|
| Nome ou número da equipe | **Alpha morreu** |
| Turma | **Informática-Subsequente** |
| Data | **22/07/2026** |

| Integrante | Papel principal | Responsabilidade |
|---|---|---|
| **Gabriel Rodrigues** | Analista | Define os requisitos, regras de acesso e critérios de validação |
| **Pedro Eduardo** | Documentador | Organiza a redação do documento e mantém o histórico das decisões |
| **Joaquim Gustavo** | Documentador / Testador | Registra os testes e valida a coerência dos resultados |
| **Ryan Cardoso** | Orçamento | Estima equipamentos, portas e margens de expansão |
| **Marcus Antônio** | Projetista-simulador | Monta a simulação e configura a topologia no Packet Tracer |

Os papéis organizam o trabalho, mas todas as decisões devem ser conferidas pela equipe como um todo.

---

## 4. Problema, objetivo e fronteira

### 4.1 Problema

O laboratório de IA precisa operar de forma segura, sem misturar o tráfego dos alunos com a rede geral do campus. Também é necessário evitar que estudantes tenham acesso indevido às máquinas de outros alunos ou às estações reservadas aos professores.

### 4.2 Objetivo

> Nossa equipe propõe uma rede para o laboratório de IA, capaz de isolar o tráfego por VLANs, controlar acessos por ACLs e manter a segurança da rede geral do campus.

### 4.3 Fronteira do trabalho

| Dentro desta atividade | Fora desta atividade de 4 aulas |
|---|---|
| requisitos e usuários | compra ou instalação de equipamentos reais |
| estimativa de máquinas e portas | orçamento, marcas e preços |
| topologia reduzida | levantamento da rede institucional real |
| IPv4 privado com redes `/24` | VLSM, IPv6, DHCP, DNS e NAT |
| VLAN, roteamento e ACL guiados | VPN ou WireGuard configurado |
| testes com `ping` ou Simple PDU | instalação real de Linux, SSH, Podman ou drivers |

---

## 5. Requisitos e permissões

| ID | Requisito obrigatório | Estratégia mínima indicada | Conferência da equipe |
|---|---|---|---|
| R01 | Separar logicamente a rede do laboratório e controlar sua comunicação com o campus. | sub-redes/VLANs e fronteira de roteamento | [X] manter |
| R02 | Impedir que um usuário comum do campus inicie acesso às máquinas do laboratório. | ACL na entrada do laboratório | [X] manter |
| R03 | Dar a professor e técnico acesso a todas as máquinas do laboratório. | contas administrativas e regras de rede autorizadas | [X] manter |
| R04 | Fazer cada estudante acessar somente a máquina atribuída. | ACL representativa + conta e chave SSH individuais | [X] manter |
| R05 | Reservar duas máquinas para uso dos professores, com acesso técnico para manutenção. | zona própria e ausência de contas estudantis | [X] manter |
| R06 | Permitir acesso remoto autenticado sem expor cada máquina diretamente à Internet. | VPN/gateway institucional e SSH | [X] manter |
| R07 | Permitir aos estudantes controlar seus próprios contêineres sem administrar o hospedeiro. | Podman *rootless* por usuário | [X] manter |
| R08 | Restringir sistema, drivers, usuários e aplicativos do hospedeiro a professor e técnico. | contas sem `sudo` para estudantes | [X] manter |
| R09 | Reservar portas e endereços para crescimento. | margem de expansão e plano IPv4 organizado | [X] manter |

### 5.1 Matriz de acesso

| Origem | Destino | Serviço ou ação | Resultado desejado | Teste relacionado |
|---|---|---|---|---|
| Professor | qualquer máquina do laboratório | administração e uso | permitir | T1 |
| Técnico | qualquer máquina e equipamento | administração e manutenção | permitir | T2 |
| Aluno-1 | apenas sua máquina atribuída | acesso local e uso do ambiente | permitir | T3 |
| Aluno-2 | apenas sua máquina atribuída | acesso local e uso do ambiente | permitir | T3 |
| Campus | nenhuma máquina do laboratório | acesso indevido | bloquear | T6 |

A permissão para o professor não contradiz o bloqueio do campus, porque o professor atua como administrador autorizado do laboratório, enquanto a rede do campus representa o acesso comum e não aprovado do ambiente.

---

## 6. Responsabilidades no Linux e contêineres

| Ação | Aluno | Professor | Técnico |
|---|:---:|:---:|:---:|
| entrar local ou remotamente na máquina atribuída | sim | sim | sim |
| acessar máquinas atribuídas a outros estudantes | não | sim | sim |
| acessar as duas máquinas exclusivas | não | sim | sim |
| baixar e construir imagens de contêiner | sim, em sua conta | sim | sim |
| criar, modificar, iniciar, parar e excluir seus contêineres | sim | sim | sim |
| criar e excluir seus volumes e redes internas de contêiner | sim | sim | sim |
| instalar bibliotecas e aplicações dentro de seus contêineres | sim | sim | sim |
| usar `sudo` no sistema hospedeiro | não | sim | sim |
| instalar ou remover Podman no hospedeiro | não | sim | sim |
| instalar drivers de GPU, alterar kernel ou sistema operacional | não | sim | sim |
| criar usuários e modificar a rede do hospedeiro | não | sim | sim |

### Diferença entre contêiner e sistema hospedeiro

Instalar uma biblioteca dentro de um contêiner é uma ação local ao ambiente isolado do usuário. Já instalar um aplicativo, driver ou alteração de kernel no sistema hospedeiro afeta toda a máquina e exige privilégio administrativo, por isso é permitido apenas para professor e técnico.

---

## 7. Dimensionamento do laboratório real

### 7.1 Usuários e equipamentos

| Item | Quantidade proposta | Como a equipe estimou? |
|---|---:|---|
| máquinas de IA para estudantes | **5** | escala reduzida para aberto em uma turma menor |
| máquinas de IA exclusivas para professores | `2` | requisito obrigatório |
| computador de administração técnica | `1` | suporte e manutenção |
| gateway/servidor de acesso remoto | `1 serviço` | pode ser uma VM ou serviço institucional existente |
| pontos de acesso sem fio | `0` | Wi-Fi não faz parte da simulação |
| outros dispositivos cabeados | `0` | sem necessidade adicional |

### 7.2 Portas de switch

| Cálculo | Resultado |
|---|---:|
| total de dispositivos cabeados | **9** |
| uplinks necessários | **1** |
| reserva de 20% | **2** |
| total de portas necessárias | **11** |
| solução escolhida: switch(es) de 24 ou 48 portas | **1 switch de 24 portas** |

### Justificativa

Um switch de 24 portas atende aos 11 acessos necessários e ainda mantém uma margem de crescimento. Essa solução é suficiente para a simulação e para uma extensão simples do laboratório.

### 7.3 Orçamento estimado da base de simulação

A seguir está o orçamento mínimo considerado para a infraestrutura de rede proposta no laboratório, com foco na simulação inicial do projeto.

| Equipamento | Modelo | Quantidade | Valor unitário (R$) | Subtotal (R$) |
|---|---|---:|---:|---:|
| Roteador | Cisco 2911 | 1 | 1.500,00 | 1.500,00 |
| Switch | Cisco 2960-24TT | 1 | 800,00 | 800,00 |
| Computador | PC básico | 2 | 2.500,00 | 5.000,00 |

| Tipo de cabo | Quantidade | Valor unitário (R$) | Subtotal (R$) |
|---|---:|---:|---:|
| Cabo Cross-Over | 1 | 30,00 | 30,00 |
| Cabo Straight-Through | 2 | 25,00 | 50,00 |

| Item | Valor (R$) |
|---|---:|
| Roteador Cisco 2911 | 1.500,00 |
| Switch Cisco 2960-24TT | 800,00 |
| 2 computadores | 5.000,00 |
| Cabos | 80,00 |
| **Total geral** | **7.380,00** |

Esse orçamento representa a infraestrutura mínima necessária para a simulação do laboratório e ajuda a dimensionar a solução em termos de custo e recursos.

---

## 8. Zonas e plano IPv4

### 8.1 Plano fixo da simulação

| Zona simulada | Rede/máscara | Gateway | Finalidade |
|---|---|---|---|
| acesso pelo campus | `10.10.10.0/24` | `10.10.10.1` | representar usuários que chegam ao roteador do laboratório |
| Equipe A | `172.20.21.0/24` | `172.20.21.1` | máquina atribuída ao Aluno-1 |
| Equipe B | `172.20.22.0/24` | `172.20.22.1` | máquina atribuída ao Aluno-2 |
| professores | `172.20.30.0/24` | `172.20.30.1` | duas máquinas exclusivas |

### 8.2 Proposta para o laboratório real

| Zona proposta | Quantidade de hosts prevista | Rede privada sugerida | O que deve acessar? |
|---|---:|---|---|
| máquinas de estudantes | **20** | **172.16.10.0/24** | as próprias máquinas e internet controlada |
| máquinas de professores | `2` | **172.16.20.0/24** | todas as máquinas de IA |
| gerenciamento | **5** | **172.16.99.0/24** | equipamentos de rede |
| acesso remoto autenticado | **50** | **172.16.200.0/24** | acesso SSH ao laboratório |

As zonas devem ficar separadas para impedir que uma falha em uma área comprometa as outras e para permitir regras de segurança mais rígidas entre grupos.

---

## 9. Arquitetura proposta

### Topologia definitiva (2 alunos)

| Equipamento | Modelo | Nome no Packet Tracer |
|---|---|---|
| Roteador | Cisco 2911 | `Roteador-Lab` |
| Switch | Cisco 2960-24TT | `Switch-Lab` |
| PC | PC-PT | `Campus` |
| PC | PC-PT | `Aluno-1` |
| PC | PC-PT | `Aluno-2` |
| PC | PC-PT | `Professor` |
| PC | PC-PT | `Tecnico` |

### Portas do Switch

| Porta | Dispositivo | VLAN |
|---|---|---|
| Gig0/1 | `Roteador-Lab` | Trunk |
| Fa0/1 | `Campus` | 10 |
| Fa0/2 | `Aluno-1` | 21 |
| Fa0/3 | `Aluno-2` | 22 |
| Fa0/4 | `Professor` | 30 |
| Fa0/5 | `Tecnico` | 30 |

### VLANs

| VLAN | Nome | Portas |
|---|---|---|
| 10 | `CAMPUS` | Fa0/1 |
| 21 | `ALUNO-1` | Fa0/2 |
| 22 | `ALUNO-2` | Fa0/3 |
| 30 | `PROFESSORES` | Fa0/4, Fa0/5 |

### Endereçamento IPv4

| Dispositivo | VLAN | IP | Gateway |
|---|---|---|---|
| `Campus` | 10 | `10.10.10.10` | `10.10.10.1` |
| `Aluno-1` | 21 | `172.20.21.10` | `172.20.21.1` |
| `Aluno-2` | 22 | `172.20.22.10` | `172.20.22.1` |
| `Professor` | 30 | `172.20.30.10` | `172.20.30.1` |
| `Tecnico` | 30 | `172.20.30.11` | `172.20.30.1` |

### Arquitetura lógica

```text
Internet / Campus
    |
    v
Roteador-Lab (subinterfaces de VLAN)
    |
    v
Switch-Lab
  ├─ VLAN 10: Campus
  ├─ VLAN 21: Aluno-1
  ├─ VLAN 22: Aluno-2
  └─ VLAN 30: Professores
```

---

## 10. Simulação guiada no Packet Tracer

### Configuração do Switch

```text
vlan 10 name CAMPUS
vlan 21 name ALUNO-1
vlan 22 name ALUNO-2
vlan 30 name PROFESSORES

interface Fa0/1
 switchport access vlan 10
interface Fa0/2
 switchport access vlan 21
interface Fa0/3
 switchport access vlan 22
interface Fa0/4
 switchport access vlan 30
interface Fa0/5
 switchport access vlan 30
interface Gig0/1
 switchport mode trunk
```

### Configuração do Roteador

```text
interface g0/0.21
 encapsulation dot1Q 21
 ip address 172.20.21.1 255.255.255.0
interface g0/0.22
 encapsulation dot1Q 22
 ip address 172.20.22.1 255.255.255.0
interface g0/0.30
 encapsulation dot1Q 30
 ip address 172.20.30.1 255.255.255.0
```

### ACLs aplicadas

```text
access-list 100 deny ip 172.20.21.0 0.0.0.255 172.20.30.0 0.0.0.255
access-list 100 deny ip 172.20.22.0 0.0.0.255 172.20.30.0 0.0.0.255
access-list 100 deny ip 172.20.21.0 0.0.0.255 172.20.22.0 0.0.0.255
access-list 100 deny ip 172.20.22.0 0.0.0.255 172.20.21.0 0.0.0.255
access-list 100 deny ip 10.10.10.0 0.0.0.255 172.20.21.0 0.0.0.255
access-list 100 deny ip 10.10.10.0 0.0.0.255 172.20.22.0 0.0.0.255
access-list 100 deny ip 10.10.10.0 0.0.0.255 172.20.30.0 0.0.0.255
access-list 100 permit ip any any

interface g0/0.21 ip access-group 100 in
interface g0/0.22 ip access-group 100 in
interface g0/0 ip access-group 100 in
```

### Sequência de validação

1. atribuir IPs fixos aos PCs;
2. configurar VLANs e trunk no switch;
3. criar subinterfaces no roteador;
4. aplicar as ACLs na entrada das subinterfaces;
5. testar `ping` e `Simple PDU` com modo Simulation;
6. registrar resultados e capturar evidências visuais.

---

## 11. Testes e evidências

| Teste | Origem | Destino | Esperado | Observado |
|---|---|---|---|---|
| T1 | `Professor` | `Aluno-1` | permitir | `Reply` |
| T2 | `Tecnico` | `Aluno-2` | permitir | `Reply` |
| T3 | `Aluno-1` | `Gateway` | permitir | `Reply` |
| T4 | `Aluno-1` | `Aluno-2` | bloquear | `Destination unreachable` |
| T5 | `Aluno-1` | `Professor` | bloquear | `Destination unreachable` |
| T6 | `Campus` | `Aluno-1` | bloquear | `Timeout` |

### Evidências

- Topologia completa: `topologia_lab.png`
- Acesso permitido: `teste_t1_permitido.png`
- Acesso bloqueado: `teste_t4_bloqueado.png`

### Diagnóstico

| Teste com problema | Causa encontrada | Correção realizada | Novo resultado |
|---|---|---|---|
| Nenhum teste apresentou falha de configuração | — | — | — |

---

## 12. Síntese da proposta

Nossa proposta cria uma rede segura para o laboratório de IA com separação lógica por VLANs, roteamento controlado e ACLs na entrada das subinterfaces. O campus fica isolado da zona acadêmica, enquanto professores e técnico mantêm acesso administrativo a todas as máquinas. Cada estudante acessa apenas a sua estação atribuída e pode gerenciar seus próprios contêineres sem elevar privilégios no sistema hospedeiro. Os testes confirmaram que o acesso autorizado funciona e o acesso indevido é bloqueado, validando a arquitetura proposta.

### Duas evoluções futuras

1. implementar acesso remoto seguro por VPN controlada;
2. adicionar um servidor de autenticação centralizado para o laboratório.

---

## 13. Registro da participação

| Integrante | Principal contribuição | O que conferiu no trabalho de outro integrante? |
|---|---|---|
| **Gabriel Rodrigues** | Definiu os requisitos e as regras de redes e acesso | Conferiu os testes e a configuração da topologia |
| **Pedro Eduardo** | Montou e organizou a documentação do projeto | Revisou as seções de dimensionamento e VLAN |
| **Joaquim Gustavo** | Registrou os testes e ajudou na revisão do documento | Conferiu o resultado dos testes de ping |
| **Ryan Cardoso** | Fez o orçamento e a estimativa de portas e equipamento | Validou a capacidade de expansão do switch |
| **Marcus Antônio** | Montou a simulação e configurou a topologia no Packet Tracer | Conferiu a consistência da documentação preenchida |

| Ferramenta | Pergunta ou tarefa solicitada | O que foi aproveitado? | Como a equipe verificou? |
|---|---|---|---|
| **Google Gemini** | "Atue como um Engenheiro de Redes Cisco..." | Comandos de configuração do switch e roteador | Testes no Packet Tracer e revisão dos resultados |

---

## 14. Checklist de entrega

* [X] O Markdown está preenchido de forma completa.
* [X] A estimativa do laboratório real inclui duas máquinas dos professores.
* [X] O cálculo de portas inclui uplink e reserva de expansão.
* [X] A simulação Packet Tracer foi montada com nomes legíveis.
* [X] Não há conflito de endereços IP na topologia.
* [X] Professor e técnico alcançam as máquinas previstas.
* [X] Os alunos acessam apenas as máquinas atribuídas.
* [X] O usuário do campus não inicia acesso ao laboratório.
* [X] Os seis testes estão registrados.
* [X] Há três evidências visuais.
* [X] Nenhum dado real ou credencial foi incluído.

---

## 15. Critérios de avaliação

| Critério | Pontos |
|---|---:|
| requisitos e matriz de permissões | 1,5 |
| dimensionamento, portas e reserva | 1,0 |
| topologia e organização das zonas | 1,5 |
| endereçamento e segmentação | 1,5 |
| regras de acesso e ACLs | 2,0 |
| seis testes e três evidências | 1,5 |
| distinção entre Linux hospedeiro e contêineres | 0,5 |
| clareza, síntese e participação da equipe | 0,5 |
| **Total** | **10,0** |

---

## Referências

CISCO NETWORKING ACADEMY. **Simulation mode: Packet Tracer tutorials**. [S. l.], [s. d.]. Disponível em: <https://tutorials.ptnetacad.net/help/default/mode_simulation.htm>. Acesso em: 22 jul. 2026.

CISCO SYSTEMS, INC. **Configure commonly used IP ACLs**. San Jose, 28 jan. 2025a. Disponível em: <https://www.cisco.com/c/en/us/support/docs/ip/access-lists/26448-ACLsamples.html>. Acesso em: 22 jul. 2026.

CISCO SYSTEMS, INC. **Configure inter VLAN routing with the use of an external router**. San Jose, 19 maio 2025b. Disponível em: <https://www.cisco.com/c/en/us/support/docs/lan-switching/inter-vlan-routing/14976-50.html>. Acesso em: 22 jul. 2026.

NATIONAL INSTITUTE OF STANDARDS AND TECHNOLOGY (NIST). **Least privilege**. Gaithersburg, [s. d.]. Disponível em: <https://csrc.nist.gov/glossary/term/least_privilege>. Acesso em: 22 jul. 2026.

OPENSSH. **OpenSSH**. Versão 10.4. [S. l.], 6 jul. 2026. Disponível em: <https://www.openssh.com/>. Acesso em: 22 jul. 2026.

PODMAN PROJECT. **podman: simple management tool for pods, containers and images**. *Podman documentation*. [S. l.], [s. d.]. Disponível em: <https://docs.podman.io/en/latest/markdown/podman.1.html>. Acesso em: 22 jul. 2026.

REKHTER, Yakov et al. **Address allocation for private internets**. RFC 1918. [S. l.]: RFC Editor, fev. 1996. Disponível em: <https://www.rfc-editor.org/rfc/rfc1918.html>. Acesso em: 22 jul. 2026.

---

> **Autoria institucional:** IFMA — Campus Itapecuru-Mirim  
> **Elaboração e docência:** Professor Thales Levi Azevedo Valente
