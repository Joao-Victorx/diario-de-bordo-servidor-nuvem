# O Dilema do Servidor em Nuvem

## 1. INTRODUÇÃO

Os sistemas operacionais fazem a ligação entre os programas e os recursos de hardware do computador. Para compreender essa relação, é importante estudar as **chamadas de sistema (System Calls)**, que permitem que os programas solicitem serviços ao núcleo do sistema operacional, e o **escalonamento de processos**, que determina como a CPU será compartilhada entre as tarefas. Este trabalho aborda esses conceitos a partir de um cenário envolvendo um servidor em nuvem, considerando também fontes de pesquisa em diferentes formatos, como vídeo, áudio e texto, conforme solicitado na atividade.

---

## 2. PARTE A – ENTENDENDO A BARREIRA DO SISTEMA (CHAMADAS DE SISTEMA)

### 2.1 Como o processo solicita essa leitura ao Sistema Operacional?

O processo de **“Geração de Relatório”** solicita a leitura por meio de uma **chamada de sistema (System Call)**. Quando precisa abrir e ler um arquivo com dados financeiros, o programa não acessa o disco diretamente. Em vez disso, ele faz uma solicitação ao Sistema Operacional. O sistema recebe essa solicitação, verifica se ela é válida e realiza o acesso ao dispositivo de armazenamento de forma controlada. Dessa forma, o Sistema Operacional funciona como uma ponte entre o programa e o hardware, garantindo que o acesso aos recursos seja realizado de maneira segura.

### 2.2 O que ocorre com o processo no momento dessa solicitação em termos de mudança de modo de acesso?

Inicialmente, o processo está executando no **Modo Usuário**, que possui acesso limitado aos recursos do computador. Quando o processo realiza uma **System Call**, ocorre uma mudança para o **Modo Kernel**. Nesse modo, o Sistema Operacional possui privilégios maiores e pode realizar operações que não são permitidas diretamente aos programas, como acessar o disco. Após o atendimento da solicitação, o resultado é devolvido ao processo e a execução retorna ao **Modo Usuário**. Essa separação entre os dois modos ajuda a proteger o sistema contra acessos indevidos ao hardware.

---

## 3. PARTE B – DIAGNOSTICANDO O ESCALONADOR

### 3.1 Por que o algoritmo FCFS está causando o “congelamento” da interface para os processos interativos?

O **FCFS (First-Come, First-Served)** executa os processos na ordem em que chegam. Como é um algoritmo **não preemptivo**, quando um processo começa a utilizar a CPU, ele normalmente continua executando até terminar ou ficar bloqueado. Nesse cenário, se um processo de geração de relatório for longo e estiver utilizando a CPU, os processos interativos da interface web precisam esperar. Como consequência, o tempo de resposta aumenta e a interface pode dar a impressão de estar **congelada**, mesmo que o sistema continue funcionando.

### 3.2 O que significa dizer que o FCFS é um algoritmo “Não Preemptivo” e como isso afeta a CPU neste cenário específico?

Dizer que o FCFS é **não preemptivo** significa que o Sistema Operacional não interrompe voluntariamente o processo que está utilizando a CPU apenas para dar oportunidade a outro processo. No cenário apresentado, um relatório que começou sua execução pode continuar ocupando a CPU enquanto os processos responsáveis pela interface web aguardam. Assim, mesmo que uma tarefa interativa precise de uma resposta rápida, ela não consegue utilizar a CPU imediatamente. Isso prejudica diretamente a **responsividade da interface**.

---

## 4. PARTE C – PROPONDO A SOLUÇÃO

### 4.1 Qual algoritmo de escalonamento seria mais adequado para resolver o problema de responsividade da interface web?

Entre **SJF, SRTN e Round-Robin**, o **Round-Robin** é uma opção adequada para esse cenário, principalmente por ser um algoritmo **preemptivo** e distribuir o tempo de CPU entre os processos. O Round-Robin utiliza uma pequena unidade de tempo chamada **quantum**. Cada processo recebe a CPU durante esse intervalo. Quando o quantum termina, o Sistema Operacional pode interromper o processo e colocá-lo novamente no final da fila, permitindo que outro processo execute. Dessa forma, um processo longo de geração de relatório não consegue monopolizar a CPU por muito tempo. Os processos interativos da interface recebem oportunidades frequentes de execução, contribuindo para diminuir o tempo de resposta e melhorar a experiência do usuário.

---

### 4.2 O que é *Starvation* e qual mecanismo pode ser usado para evitá-la?

**Starvation**, ou **inanição**, ocorre quando um processo permanece esperando por muito tempo para ser executado porque outros processos continuam recebendo prioridade para utilizar a CPU. Em um sistema baseado em prioridades, esse problema pode acontecer com os processos de relatório caso a interface web tenha prioridade máxima e esteja constantemente gerando novas tarefas. Para evitar esse problema, o Sistema Operacional pode utilizar o mecanismo chamado **aging (envelhecimento)**. Nesse mecanismo, a prioridade de um processo que permanece esperando durante muito tempo aumenta gradualmente. Dessa forma, mesmo os processos que inicialmente possuem prioridade menor conseguem, depois de determinado período, receber a CPU.

---

## 5. INSTRUMENTO VISUAL 

O diagrama abaixo apresenta a relação entre os principais conceitos abordados na pesquisa, desde a solicitação de serviços por meio das chamadas de sistema até o escalonamento, a preempção e a troca de contexto.

#### Figura 1 – Relação entre chamadas de sistema e escalonamento e escalonamento

<img width="571" height="641" alt="Diagrama " src="https://github.com/user-attachments/assets/6613d87e-9ab7-42d5-8f28-128b762fa439" />

---

## REFERÊNCIAS

DICKEN, Ben. How Operating Systems ACTUALLY work (kernel mode vs. user mode). YouTube, 13 jul. 2026. Disponível em: https://www.youtube.com/watch?v=2beOYY4S0B8. Acesso em: 1 out. 2026.

MIT – MASSACHUSETTS INSTITUTE OF TECHNOLOGY. Operating Systems Lecture Notes: Lecture 6 – CPU Scheduling. [S. l.], [s. d.]. Disponível em: https://people.csail.mit.edu/rinard/teaching/osnotes/h6.html. Acesso em: 1 out. 2026.

NESO ACADEMY. System Calls. YouTube, 15 mar. 2018. Disponível em: https://www.youtube.com/watch?v=lhToWeuWWfw. Acesso em: 1 out. 2026.

OPERATING SYSTEMS CRASHCASTS. Unlocking Efficiency: Essential Scheduling Criteria for Smarter Planning. Podcast, 11 out. 2024. Disponível em: https://open.spotify.com/show/2fzweAsP82otqYiKupKoIg. Acesso em: 1 out. 2026.

UNIVERSITY OF WISCONSIN–MADISON. CS 537 Notes, Section #11: Scheduling and CPU Scheduling. [S. l.], [s. d.]. Disponível em: https://pages.cs.wisc.edu/~bart/537/lecturenotes/s11.html. Acesso em: 1 out. 2026.
