# trabalho-final-sm66a.
Nesse jogo um número aleatório é gerado e o usuário deve tentar adivinhar o número gerado vendo o número atual; a cada resposta do usuário, um novo número é gerado.
# Título do Projeto: Jogo de azar atrelado à tempo de reação 

## 📝 Descrição Técnica

Este repositório contém o desenvolvimento de um sistema embarcado para diversão. O projeto foi concebido como parte dos requisitos avaliativos da disciplina SM66A - Sistemas Microcontrolados.

## ✨ Funcionalidades Implementadas (mínimo de 1)

- Comunicação de interfaces

## ✨ Periféricos Utilizados (mínimo de 4)

- GPIO_PortF (LED)
- UART
- Clock
- Timer
- EnableInterrupts


## 🛠️ Hardware e Componentes (mínimo 1)

* Microcontrolador: TM4C123G
* Outros componentes: Computador (teclado e monitor)

## ⚙️ Procedimento de Montagem e Execução

Conexão do Microcontrolador ao computador via USB.

1.  **Configuração do Ambiente:** Detalhamento das bibliotecas e configurações da IDE necessárias.
Bibliotecas utilizadas:
randomx.h - utilzado para definir a variável X, para incrementar um valor aleatório;
Timer0A.h - utilizado para configurar e inicializar o timer do sistema;
tm4c123gh6pm.h - utilizada como biblioteca geral do Microcontrolador;
UART.h - utilizada para configuração e inicialização do periférico UART;


## 🚀 Cronograma e Evolução (Roadmap)

- [ ] Definição da arquitetura de software.
- [ ] Implementação do módulo de leitura de sensores.
- [ ] Implementação da lógica de controle.
- [ ] Validação e testes funcionais.

