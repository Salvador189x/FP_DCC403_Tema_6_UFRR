# FP_DCC403_Tema_6_UFRR

## Desenvolvimento de uma Política de Escalonamento para Tarefas em Segundo Plano no Kernel Linux

### Disciplina

DCC403 – Sistemas Operacionais

### Instituição

Universidade Federal de Roraima (UFRR)

### Autor

**Salvador de Jesus Malavé Campos**
**Matrícula:** 2022008214

---

# Resumo

Este projeto apresenta o desenvolvimento de uma nova política de escalonamento para o Kernel Linux denominada **SCHED_BACKGROUND**, destinada à execução de tarefas de segundo plano com prioridade inferior às classes nativas do sistema operacional.

A proposta visa minimizar a interferência de tarefas auxiliares em aplicações críticas, permitindo que processos classificados como SCHED_BACKGROUND utilizem a CPU apenas quando não existirem tarefas aptas pertencentes às demais classes de escalonamento do sistema.

O trabalho contempla o estudo da infraestrutura de escalonamento do Linux, a análise das estruturas internas do kernel, a implementação da nova política, o desenvolvimento de um utilitário de linha de comando para gerenciamento da política e a realização de testes experimentais para avaliação de desempenho.

---

# Objetivos

## Objetivo Geral

Implementar uma nova política de escalonamento denominada **SCHED_BACKGROUND** no Kernel Linux, destinada ao gerenciamento de tarefas de segundo plano.

## Objetivos Específicos

* Estudar a arquitetura interna do escalonador do Kernel Linux.
* Analisar a estrutura `task_struct`.
* Compreender o funcionamento da hierarquia de classes de escalonamento.
* Implementar uma nova política de escalonamento.
* Criar um utilitário de usuário para alteração da política de processos.
* Desenvolver programas de benchmark para avaliação quantitativa.
* Avaliar o impacto da nova política sobre o desempenho do sistema.

---

# Problema

Em ambientes modernos de computação em nuvem, data centers e sistemas distribuídos, aplicações críticas compartilham recursos computacionais com tarefas secundárias, como:

* Compactação de logs;
* Processamento estatístico;
* Sincronização de dados;
* Backup;
* Monitoramento de infraestrutura;
* Rotinas administrativas.

Mesmo executando com prioridades reduzidas, essas tarefas continuam competindo pelos ciclos de CPU, podendo introduzir degradação de desempenho e aumento de latência para aplicações críticas.

---

# Solução Proposta

A solução consiste na implementação de uma nova política de escalonamento denominada:

```c
SCHED_BACKGROUND
```

Processos classificados nesta política somente recebem tempo de CPU quando não existirem tarefas prontas pertencentes às classes tradicionais do sistema.

A prioridade entre classes passa a obedecer ao seguinte modelo:

```text
SCHED_FIFO
SCHED_RR
SCHED_OTHER
SCHED_BATCH
SCHED_IDLE
SCHED_BACKGROUND
```

Dessa forma, tarefas secundárias permanecem completamente isoladas da execução das aplicações prioritárias.

---

# Estrutura do Projeto

```text
FP_DCC403_Tema_6_UFRR
│
├── README.md
│
├── relatorio/
│   └── artigo_tema6.pdf
│
├── benchmark/
│   └── benchmark.c
│
├── utilitario/
│   └── setbg.c
│
├── kernel_patch/
│   └── observacoes_kernel.md
│
├── patches/
│   └── sched_background.patch
│
└── imagens/
```

---

# Arquivos Modificados no Kernel

Os principais arquivos envolvidos na implementação da política SCHED_BACKGROUND são:

```text
include/uapi/linux/sched.h
kernel/sched/core.c
kernel/sched/sched.h
kernel/sched/background.c
kernel/sched/Makefile
```

---

# Utilitário de Linha de Comando

Foi desenvolvido um utilitário denominado:

```text
setbg
```

Sua finalidade é alterar a política de escalonamento de um processo para SCHED_BACKGROUND.

Exemplo de utilização:

```bash
./setbg <PID>
```

Exemplo:

```bash
./setbg 1234
```

---

# Benchmark

O benchmark desenvolvido realiza medições de:

* Tempo de usuário (User Time);
* Tempo de sistema (System Time);
* Tempo total de execução;
* Tempo de parede (Wallclock).

As métricas são obtidas por meio das chamadas:

```c
gettimeofday()
getrusage()
```

conforme especificado no enunciado do projeto.

---

# Ambiente Experimental

## Sistema Operacional

Ubuntu 26.04

## Kernel Base

Linux Kernel 6.1.161

## Máquina Virtual

Oracle VirtualBox

## Ferramentas Utilizadas

* GCC
* GNU Make
* Git
* Linux Kernel Source
* Ubuntu
* VirtualBox

---

# Resultados Esperados

A implementação da política SCHED_BACKGROUND busca:

* Reduzir interferências em aplicações críticas;
* Melhorar a previsibilidade temporal;
* Aumentar o aproveitamento dos ciclos ociosos da CPU;
* Diminuir a contenção entre processos de diferentes níveis de prioridade;
* Melhorar a eficiência energética em ambientes com múltiplas cargas de trabalho.

---

# Compilação

Compilação do Kernel:

```bash
make -j4
```

Instalação:

```bash
sudo make modules_install
sudo make install
```

Atualização do GRUB:

```bash
sudo update-grub
```

Reinicialização:

```bash
sudo reboot
```

---

# Testes

Os testes foram realizados utilizando processos concorrentes executando tarefas intensivas em CPU.

Foram comparados cenários utilizando:

* Escalonamento padrão do Linux;
* Escalonamento SCHED_BACKGROUND.

As métricas coletadas permitiram avaliar o impacto da nova política sobre o desempenho das aplicações prioritárias.

---

# Conclusão

O desenvolvimento da política SCHED_BACKGROUND representa uma abordagem para isolamento de cargas de trabalho de baixa prioridade em sistemas Linux. A solução proposta busca reduzir a interferência de tarefas auxiliares em aplicações críticas, contribuindo para maior previsibilidade temporal e melhor aproveitamento dos recursos computacionais disponíveis.

---

# Declaração de Uso de Inteligência Artificial

Durante o desenvolvimento deste trabalho foi utilizada a ferramenta ChatGPT (OpenAI) como suporte à revisão textual, organização estrutural da documentação, esclarecimento de conceitos relacionados ao Kernel Linux, compreensão do subsistema de escalonamento de processos e auxílio na elaboração inicial de trechos de código-fonte e scripts experimentais.

A ferramenta foi utilizada exclusivamente como mecanismo de apoio técnico e educacional. Todas as decisões de projeto, adaptações do código, validações experimentais, testes realizados e conteúdo final apresentado foram analisados, revisados e validados pelo autor.

O autor assume integral responsabilidade pela correção técnica, integridade acadêmica e conteúdo final deste trabalho.

---

# Referências

LOVE, Robert. Linux Kernel Development. Addison-Wesley, 2010.

BOVET, Daniel P.; CESATI, Marco. Understanding the Linux Kernel. O'Reilly Media, 2005.

CORBET, Jonathan; RUBINI, Alessandro; KROAH-HARTMAN, Greg. Linux Device Drivers. O'Reilly Media, 2005.

Linux Kernel Documentation. Disponível em: https://www.kernel.org

Kernel.org Source Repository. Disponível em: https://cdn.kernel.org
