# Relatório do Projeto de Simulação da Bolsa de Valores

### Alunos: 
- Bruno Pergher
- William Pieri
- Vítor Farias 

### Professores: 
- Fábio Pinheiro
- Manasses Ribeiro

### Data: 
26/08/2024

---

## Introdução

O mercado financeiro exige sistemas que sejam não apenas eficientes, mas também seguros, escaláveis e capazes de lidar com transações complexas em tempo real. A construção de uma infraestrutura tecnológica que atenda a essas necessidades envolve uma série de desafios, desde a manipulação básica de processos concorrentes até a implementação de mecanismos avançados de sincronização e escalabilidade.

Neste projeto, nosso objetivo é desenvolver um sistema financeiro completo em etapas progressivas, começando com uma prova de conceito simples e culminando em uma solução robusta que integra as mais modernas ferramentas e práticas de desenvolvimento. Através de um roadmap dividido em três versões distintas, cada uma com seus próprios objetivos e requisitos, buscamos construir uma base sólida que evolua para um sistema capaz de operar em ambientes de alta demanda e complexidade.

## Fundamentação Teórica

O mercado financeiro lida com volumes massivos de transações diariamente, onde cada operação, seja de compra ou venda de ativos, deve ser executada com total garantia de integridade e confiabilidade. Um sistema financeiro robusto precisa lidar com a concorrência de múltiplos usuários, garantir que as transações sejam correspondidas corretamente, e manter a consistência dos dados em todas as operações. Abaixo, exploramos conceitos fundamentais como concorrência, sincronização, atomicidade e consistência, que são essenciais para a construção de um sistema robusto e escalável. Esses princípios guiam o desenvolvimento de mecanismos que asseguram a correta execução das transações financeiras e a integridade dos dados.

### 1. Concorrência e Sincronização

A concorrência é um conceito central em sistemas paralelos, onde várias operações podem ocorrer simultaneamente. Em um ambiente de mercado financeiro, múltiplos usuários podem tentar realizar operações de compra e venda de ações ao mesmo tempo. Para evitar inconsistências e garantir a execução correta das transações, é necessário sincronizar o acesso aos recursos compartilhados, como o inventário de ações. Ferramentas de sincronização, como mutexes (travas) e semáforos, são fundamentais para evitar condições de corrida (race conditions), onde duas ou mais operações simultâneas tentam modificar o mesmo recurso, levando a resultados incorretos.

### 2. Atomicidade e Consistência Transacional

Transações financeiras exigem que as operações sejam atômicas, ou seja, que sejam executadas de maneira completa e indivisível. No contexto de compra e venda de ações, a atomicidade assegura que, se uma parte da transação falhar (por exemplo, se não houver ações suficientes para completar uma compra), a transação inteira seja revertida, mantendo o sistema em um estado consistente. A consistência transacional é mantida através das propriedades ACID (Atomicidade, Consistência, Isolamento e Durabilidade), garantidas por sistemas de gerenciamento de banco de dados transacionais.

### 3. Matching Engine (Motor de Correspondência)

No mercado financeiro, o sistema de correspondência de ordens (matching engine) é responsável por parear ordens de compra e venda. Esse processo não é trivial e envolve encontrar pares de ordens que satisfaçam critérios como preço, quantidade e prioridade temporal. Um matching engine deve ser capaz de gerenciar de forma eficiente um grande número de ordens simultâneas, garantindo que cada transação ocorra somente quando uma contrapartida válida é encontrada. Este mecanismo é fundamental para a operação de mercados financeiros, garantindo a liquidez e a integridade das transações.

### 4. Consistência de Dados e Gestão de Estados

A consistência de dados é crucial em sistemas financeiros, onde qualquer discrepância pode levar a grandes prejuízos. Em um sistema de transações financeiras, é essencial que o estado do sistema (por exemplo, o saldo de ações de um usuário) seja atualizado de maneira precisa e confiável após cada transação. Isso requer um mecanismo robusto de gestão de estados, que garanta que todas as alterações sejam corretamente registradas e persistidas.

### 5. Segurança e Confiabilidade

Além da consistência e sincronização, sistemas financeiros devem ser seguros e confiáveis. Isso inclui a proteção contra fraudes, ataques cibernéticos e falhas sistêmicas. A segurança deve ser incorporada em todas as camadas do sistema, desde o controle de acesso até a criptografia de dados sensíveis. A confiabilidade, por sua vez, garante que o sistema esteja sempre disponível para processar transações, minimizando o tempo de inatividade e garantindo a continuidade dos negócios.

### 6. Escalabilidade

Os sistemas financeiros devem ser escaláveis para lidar com volumes crescentes de transações, especialmente durante períodos de alta volatilidade nos mercados. A escalabilidade envolve a capacidade de adicionar recursos (como mais servidores ou instâncias de processamento) sem comprometer o desempenho do sistema. O modelo de produtores e consumidores, embora simples e eficiente para muitos cenários, pode enfrentar desafios de escalabilidade no contexto financeiro, onde a demanda pode variar drasticamente.

### 7. Garantia de Ordem de Execução

Em operações financeiras, a ordem de execução das transações pode ser crucial, especialmente quando se trata de ordens de mercado, onde a prioridade temporal é determinante. Um sistema robusto deve garantir que as ordens sejam executadas na ordem correta, conforme sua chegada, e que o processo seja transparente para os usuários.

## Utilização dos Containers

- **RabbitMQ**: Gerencia a comunicação entre componentes através de quatro filas principais:
  - **Ordens de Compras**: Recebe todas as ordens de compra emitidas pelos clientes compradores.
  - **Ordens de Vendas**: Recebe todas as ordens de venda emitidas pelos clientes vendedores.
  - **Resultados de Compras**: Armazena e disponibiliza os resultados das ordens de compra após serem processadas.
  - **Resultados de Vendas**: Armazena e disponibiliza os resultados das ordens de venda após serem processadas.
- **Comprador**: Simula múltiplos clientes enviando ordens de compra de ações para o sistema, interagindo com a fila de ordens de compras do RabbitMQ.
- **Vendedor**: Simula múltiplos clientes enviando ordens de venda de ações para o sistema, interagindo com a fila de ordens de vendas do RabbitMQ.
- **Corretora**: Consome as ordens emitidas por seus clientes, encaminha para a bolsa de valores e distribui os resultados das transações de volta aos clientes.
- **Bolsa**: Responsável por consumir as ordens de compra e venda das filas, processar as transações de forma segura e eficiente, e retornar os resultados para as filas correspondentes de resultados.

## Objetivos Esperados

Nosso projeto tem como objetivo principal desenvolver um sistema financeiro robusto e eficiente, capaz de lidar com as complexidades e desafios inerentes a transações em larga escala. O desenvolvimento será realizado em três versões distintas, cada uma com metas e requisitos específicos que progressivamente abordam as necessidades críticas do sistema.

### 1ª Versão: Prova de Conceito com Threads
- Desenvolver uma solução simples que sirva como prova de conceito (PoC), utilizando apenas threads para demonstrar a viabilidade do sistema.
- Elaborar um relatório que apresente um resumo da abordagem adotada e inclua um diagrama que exemplifique a solução proposta.
- Implementar a PoC em uma linguagem de programação escolhida pela equipe, com foco na implementação básica dos produtores e consumidores.
- Avaliar o desempenho da solução proposta em um cenário de uso em larga escala, verificando se a PoC atende às necessidades iniciais do projeto.

### 2ª Versão: Sincronização e Controle de Condição de Corrida
- Avançar para uma solução que incorpore os conceitos de sincronização, abordando tanto a cooperação quanto a competição entre threads, e tratando condições de corrida (race conditions) e seções críticas.
- Empacotar os dados em formato JSON para facilitar o processamento e a transmissão.
- Utilizar filas para gerenciar o fluxo de entrada e saída de dados, garantindo que as transações sejam processadas na ordem correta.
- Implementar mecanismos de sincronização para evitar condições de corrida e controlar as seções críticas, assegurando a integridade e consistência dos dados.

### 3ª Versão: Solução Completa e Escalável
- Desenvolver uma solução completa que aplique toda a teoria aprendida ao longo do curso, além de integrar ferramentas e bibliotecas avançadas.
- Garantir a escalabilidade do sistema, preparando-o para lidar com um aumento significativo no volume de transações.
- Implementar o uso de message brokers, como RabbitMQ ou Apache Kafka, para melhorar a eficiência e confiabilidade no processamento de mensagens e transações.
- Utilizar containers Docker para encapsular os componentes do sistema, facilitando a implantação e garantindo a portabilidade. A orquestração dos containers será realizada utilizando Docker Compose.

## Resultados e Discussão

A primeira versão do nosso projeto feita em GO, que é uma linguagem mais adequada ao paralelismo em comparação às mais utilizadas, consiste na implementação de uma solução simples utilizando apenas o gerenciamento de threads, projetada para servir como uma prova de conceito (PoC). Espera-se que essa PoC permita uma avaliação inicial da viabilidade do sistema em termos de processamento básico de transações financeiras, utilizando um padrão de produtores e consumidores.

![diagrma](https://github.com/user-attachments/assets/8892c2c3-0bcf-485a-9c00-3c9e14ab34ce)

### Visão Geral do Código

O código é um exemplo de um sistema de produtores e consumidores implementado em GO. O objetivo é demonstrar como múltiplos produtores podem gerar tarefas que são processadas por múltiplos consumidores, utilizando canais para comunicação e sincronização entre goRoutines.

#### Objetivo Principal:
- Criar um ambiente onde vários produtores geram tarefas que são colocadas em uma fila (canal).
- Vários consumidores retiram e processam essas tarefas.

#### Produtores e Consumidores:
- **Produtores**: Cada produtor cria tarefas em intervalos aleatórios e as envia para uma fila compartilhada.
- **Consumidores**: Cada consumidor retira tarefas da fila e as processa, com o tempo de processamento também sendo aleatório.

#### Sincronização e Controle:
- **Comunicação**: Produtores e consumidores se comunicam através de canais (taskQueue para tarefas e stopChan para sinalizar a parada).
- **Sincronização**: O uso de `WaitGroup` garante que o programa só finalize quando todos os produtores e consumidores tenham terminado suas operações.

#### Fluxo de Execução:
- **Inicialização**: O programa inicializa os produtores e consumidores como goRoutines.
- **Execução**: Os produtores criam e enviam tarefas para a fila, enquanto os consumidores processam essas tarefas.
- **Parada e Finalização**: Após um tempo predeterminado, o canal de parada (stopChan) é fechado, sinalizando aos produtores para que eles parem de criar novas tarefas. O canal de tarefas (taskQueue) é então fechado, permitindo que os consumidores finalizem sua execução após processar todas as tarefas restantes.

### Aspectos de Concorrência:
O código utiliza goRoutines para realizar operações concorrentes. Cada produtor e consumidor funciona de maneira independente e simultânea, mas o uso de canais e `WaitGroup` garante que a execução seja coordenada e que todos os processos sejam corretamente finalizados.

### Resultados Positivos Esperados
- **Funcionamento Básico**: O sistema deverá ser capaz de demonstrar o funcionamento básico de um modelo de produtores e consumidores, onde múltiplos produtores geram tarefas (transações) e múltiplos consumidores as processam.
- **Operação em Larga Escala**: Espera-se que o sistema suporte um volume considerável de transações, simulando um ambiente de produção com um número elevado de operações simultâneas.

### Limitações Identificadas
Apesar de atender aos requisitos iniciais como prova de conceito, a solução apresentada possui limitações significativas que a tornam inadequada para um ambiente financeiro real:

- **Falta de Sincronização**: O código atual não implementa mecanismos de sincronização, o que pode levar a condições de corrida (race conditions), onde múltiplas threads acessam e modificam dados compartilhados simultaneamente, resultando em inconsistências.
- **Ausência de Controle de Concorrência**: O modelo básico de threads não oferece garantias de que as transações serão processadas em uma ordem que preserve a integridade dos dados, o que é crítico em operações financeiras.
- **Não Escalabilidade**: Embora o sistema possa suportar um número elevado de transações, ele não foi projetado para escalar de maneira eficiente. Em um ambiente de produção, aumentos no volume de transações podem resultar em problemas de desempenho e falta de robustez.
- **Sem Garantia de Consistência**: A solução atual não inclui mecanismos para garantir a consistência dos dados em caso de falhas ou interrupções, o que é essencial em um sistema financeiro.
