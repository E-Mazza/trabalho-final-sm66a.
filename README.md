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

2.  **Compilação e Upload:** Instruções para compilar o firmware e transferi-lo para o microcontrolador.
#include <stdint.h>
#include <stdbool.h>
#include "tm4c123gh6pm.h"
#include "randomx.h"

int entrada, contador, errou=0, acabou=0;

void Clock_Init(long SYSDIV_var);
void UART_Init();
void PortF_Init();
void Timer0A_Init();
void EnableInterrupts();

int main(void) {
    Clock_Init(4);
    UART_Init();
    PortF_Init();
    Timer0A_Init();
    while (1) {
        UART_OutString("Bem vindo ao Acerte o numero  \n\r");
        UART_OutString("Digite um numero de 0 a 10 \n\r");
        entrada = UART_InUDec();
        UART_OutString("\n\r");
        if (entrada == X) {
            UART_OutString("Parabens voce acertou\n\r");
            GPIO_PORTF_DATA_R |= 0x0C;
            GPIO_PORTF_DATA_R &= ~0x02;
            break;
        }
        else {
            UART_OutString("Voce errou tente em 5 segundos\n\r");
            errou=1;
            GPIO_PORTF_DATA_R |= 0x02;
            GPIO_PORTF_DATA_R &= ~0x0C;

        }

        if(errou==1){
                TIMER0_CTL_R = 0x01;
                contador=0;
            while(contador<50000){
                if(acabou==1){
                   TIMER0_CTL_R = 0x01;
                   acabou=0;
                   contador++;
                }

            }
        }
        errou==0;


    }
}
void PortF_Init(void){ volatile unsigned long delay;
  SYSCTL_RCGC2_R |= 0x00000020;     // 1) F clock
  delay = SYSCTL_RCGC2_R;           // delay
  GPIO_PORTF_LOCK_R = 0x4C4F434B;   // 2) unlock PortF PF0
  GPIO_PORTF_CR_R = 0x1F;           // allow changes to PF4-0
  GPIO_PORTF_AMSEL_R = 0x00;        // 3) disable analog function
  GPIO_PORTF_PCTL_R = 0x00000000;   // 4) GPIO clear bit PCTL Port Control
  GPIO_PORTF_DIR_R = 0x0E;          // 5) PF4,PF0 input, PF3,PF2,PF1 output
  GPIO_PORTF_AFSEL_R = 0x00;        // 6) no alternate function
  GPIO_PORTF_PUR_R = 0x11;          // enable pullup resistors on PF4,PF0
  GPIO_PORTF_DEN_R = 0x1F;          // 7) enable digital pins PF4-PF0
}

void Clock_Init(long SYSDIV_var){
  // a) configure the system to use RCC2 for advanced features
  SYSCTL_RCC2_R |= 0x80000000;
  // b) bypass PLL e SYSDIV while initializing
  SYSCTL_RCC2_R |= 0x00000800;
  SYSCTL_RCC_R &= ~0x00400000;
  // c) select the crystal value and oscillator source
  SYSCTL_RCC_R &= ~0x000007C0;
  SYSCTL_RCC_R += 0x00000540;
  SYSCTL_RCC2_R &= ~0x00000070;
  SYSCTL_RCC2_R += 0x00000000;
  // d) activate PLL by clearing PWRDN
  SYSCTL_RCC2_R &= ~0x00002000;
  // e) set the desired system divider and the system divider least significant bit
  SYSCTL_RCC2_R |= 0x40000000;
  SYSCTL_RCC2_R = (SYSCTL_RCC2_R&~0x1FC00000)+ (SYSDIV_var<<22);
  SYSCTL_RCC_R |= 0x00400000;
  // f) wait for the PLL to lock by polling PLLLRIS
  while((SYSCTL_RIS_R&0x00000040)==0){};
  // g) enable use of PLL by clearing BYPASS
  SYSCTL_RCC2_R &= ~0x00000800;
}

void Timer0A_Init(uint32_t period){

  SYSCTL_RCGCTIMER_R |= 0x01; // 0) activate TIMER0
  TIMER0_CTL_R = 0x00;  // 1) disable TIMER0A during setup
  TIMER0_CFG_R = 0x04;  // 2) configure for 16-bit mode
  TIMER0_TAMR_R = 0x01; // 3) configure for one-shot mode
  //down-count settings
  TIMER0_TAILR_R = period-1;  // 4) reload value
  TIMER0_TAPR_R = 255;        // 5) bus clock resolution
  TIMER0_IMR_R = 0x01; // mask
  NVIC_PRI4_R = 0x60000000; //priority 4
  NVIC_EN0_R |= 1<<19;

}


void Timer0A_Handler(void){
acabou=1;
TIMER0_ICR_R = 0x01;
}

## 🚀 Cronograma e Evolução (Roadmap)

- [ ] Definição da arquitetura de software.
- [ ] Implementação da lógica de controle.
- [ ] Validação e testes funcionais.
