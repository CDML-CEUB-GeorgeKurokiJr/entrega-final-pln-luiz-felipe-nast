
# 🚦 Projeto PLN - Como funciona o fluxo?

###  1️⃣ O Classificador de Texto (Text Classifier)

Primeiro, o sistema usa um modelo de IA para **entender que tipo de pergunta o usuário fez**.
Ele escolhe uma dessas categorias:

| Categoria            | Explicação simples                                                                     |
| -------------------- | -------------------------------------------------------------------------------------- |
| **kernel_module**    | Perguntas práticas sobre como criar, programar ou consertar um módulo do kernel Linux. |
| **kernel_general**   | Perguntas sobre o kernel em si (memória, interrupções, processos, boot etc.).          |
| **setup_tools**      | Perguntas sobre ferramentas e ambiente (GCC, Makefile, QEMU, debugging…).              |
| **kernel_module_qa** | Perguntas teóricas sobre o que são módulos, para que servem etc.                       |
| **out_of_question**  | Perguntas que não têm nada a ver com kernel.                                           |

👉 Se a pergunta cair em **out_of_question**, o fluxo manda uma resposta simples:
**“Pergunta fora do tema.”**

---

###  2️⃣ Encaminhamento para o Agente Correto

Se a pergunta for válida, ela é enviada para o agente certo:

#### 🔧 kernel_module

Agente especializado em **programação de módulos**.
Saída típica: código C, instruções de init/exit, macros, erros de build, estrutura do driver etc.

Esse agente ainda tem **duas ferramentas internas**:

* **kernel_module_code** → ganha destaque quando a pergunta é sobre implementação
* **kernel_module_error** → ganha destaque quando há um erro real (undefined symbol, dmesg, modprobe, warnings etc.)

#### 🧠 kernel_general

Agente para perguntas sobre o **funcionamento interno do kernel Linux**:
memory management, system calls, interrupções, processos, scheduling, boot, GDT/IDT…

#### 🛠️ setup_tools

Agente para perguntas sobre o **ambiente de desenvolvimento**:
GCC, Makefile, toolchains, QEMU, debugging, dependências…

#### 📘 kernel_module_qa

Agente para perguntas **teóricas** sobre módulos:
o que são, como funcionam, para que servem, vantagens, arquitetura etc.
(Não fornece código — só explicação.)

---

### 🧩 Resumo geral do funcionamento

1. O usuário envia uma pergunta.
2. O Text Classifier decide a categoria.
3. A pergunta vai para o agente certo.
4. Se for módulo, o agente ainda decide entre ferramenta de código ou de erro.
5. O agente consulta sua biblioteca no Qdrant.
6. O sistema responde ao usuário.

