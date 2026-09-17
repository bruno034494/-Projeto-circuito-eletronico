# Sistema Digital de Controle de Iluminação Residencial

Projeto prático de circuito digital combinacional desenvolvido no **Autodesk Tinkercad** para a disciplina de **Sistemas Digitais** do curso de Licenciatura em Ciências da Computação do **IF Baiano — Campus Senhor do Bonfim**, sob orientação do **Prof. Jesse Nery Filho**.

---

##  Visão Geral do Projeto

O sistema realiza o controle lógico de dois pontos de iluminação residencial a partir de quatro interruptores manuais, relacionando expressões booleanas, simplificação por Mapa de Karnaugh e montagem física com circuitos integrados TTL/CMOS e transistores de potência.

### Cenário Operacional
* **Entradas (Interruptores):**
  * `A`: Interruptor da Sala
  * `B`: Interruptor da Cozinha
  * `C`: Interruptor do Quintal
  * `D`: Interruptor da Garagem
* **Saídas (Lâmpadas):**
  * `S1` (**Hall Central**): Acende se a Sala e a Cozinha forem acionadas simultaneamente ($A \cdot B$) **OU** se o Quintal e a Garagem forem acionados simultaneamente ($C \cdot D$).
  * `S2` (**Iluminação Externa Complementar**): Acende exclusivamente se o Quintal e a Garagem forem acionados simultaneamente ($C \cdot D$).

> **Regra de Dependência:** Sempre que a iluminação externa ($S2$) estiver ligada, o hall central ($S1$) também acenderá ($S2 = 1 \implies S1 = 1$). O inverso não é obrigatório.

---

##  Expressões Booleanas e Otimização

### 1. Saída $S1$ (Hall Central)
* **Forma Canônica (Mintermos):**  
  $$S1 = \Sigma m(3, 7, 11, 12, 13, 14, 15)$$
* **Expressão Simplificada (Karnaugh):**  
  $$S1 = (A \cdot B) + (C \cdot D)$$

### 2. Saída $S2$ (Externa Complementar)
* **Forma Canônica (Mintermos):**  
  $$S2 = \Sigma m(3, 7, 11, 15)$$
* **Expressão Simplificada:**  
  $$S2 = C \cdot D$$

### 3. Otimização de Portas Lógicas
A saída da porta **AND** responsável por $C \cdot D$ é compartilhada: alimenta simultaneamente uma das entradas da porta **OR** de $S1$ e o estágio de acionamento de $S2$. Dessa forma, todo o sistema é implementado utilizando apenas **duas portas AND** e **uma porta OR**.

---

##  Tabela-Verdade Resumida (Casos de Teste)

| Entrada (ABCD) | Cenário de Entrada | S1 (Hall Central) | S2 (Externa) | Estado |
| :---: | :--- | :---: | :---: | :---: |
| `0000` | Todas as chaves desligadas (repouso) | `0` (Apagada) | `0` (Apagada) | Conforme |
| `1100` | Sala e Cozinha ativadas | `1` (Acesa) | `0` (Apagada) | Conforme |
| `0011` | Quintal e Garagem ativados | `1` (Acesa) | `1` (Acesa) | Conforme |
| `1111` | Todas as entradas ativadas | `1` (Acesa) | `1` (Acesa) | Conforme |
| `1010` | Pares incompletos (Sala e Quintal) | `0` (Apagada) | `0` (Apagada) | Conforme |

---

##  Lista de Componentes (BOM)

| Componente | Identificação | Função |
| :--- | :---: | :--- |
| **Placa de ensaio (Protoboard)** | - | Montagem e barramento dos circuitos |
| **Fonte DC (5.00 V)** | `P2` | Alimentação lógica e de potência |
| **DIP Switch 4 vias (DPST)** | `SW1` | Simulação das entradas manuais $A, B, C, D$ |
| **CI 74HC08** | `U1` | 4 portas lógicas AND de 2 entradas |
| **CI 74HC32** | `U2` | 4 portas lógicas OR de 2 entradas |
| **Transistores MOSFET Canal N (nMOS)** | `T1, T2` | Chaveamento de potência das lâmpadas |
| **Lâmpadas incandescentes** | `L1, L3` | Indicadores visuais das saídas $S1$ e $S2$ |
| **Resistores de 10 kΩ** | `R1-R5, R7` | Resistores de *pull-down* para as chaves |
| **Resistores de 200 kΩ** | `R6, R8` | Polarização e proteção de *gate* dos MOSFETs |
| **Capacitores de 100 nF** | `C1, C2` | Desacoplamento de alimentação dos CIs |

---

##  Estrutura do Repositório

```text
├── PROJETO DE SISTEMAS DIGITAIS.pdf   # Diagrama esquemático gerado no Tinkercad
├── PROJETO DE SISTEMAS DIGITAIS.png   # Imagem da montagem física na protoboard
├── Relatorio_Sistemas_Digitais_final.pdf # Relatório técnico completo do trabalho
├── bom.csv                            # Lista detalhada de materiais (Bill of Materials)
├── apresentação.mp4                   # Demonstração e explicação em vídeo do circuito
└── README.md                          # Documentação do projeto
