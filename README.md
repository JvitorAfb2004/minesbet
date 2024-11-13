
---

# Mines Bet

## Descrição

Este projeto foi desenvolvido com o intuito de testar e aprimorar meus conhecimentos em Vue.js e Vuetify, criando uma interface interativa e dinâmica para um jogo simples de cassino baseado no conceito de "minas". A proposta do jogo é apostar um valor, onde você deve clicar em botões representando células de um tabuleiro. Se você clicar em uma célula que não tem bomba, o valor da sua aposta é multiplicado com base no número de "minas" restantes. Caso clique em uma célula com bomba, o valor apostado é descontado do seu saldo.

## Funcionalidades

- **Apostar um valor**: O jogador aposta um valor que é usado para multiplicar os ganhos, dependendo do número de bombas restantes.
- **Aumento dos ganhos**: Cada célula sem bomba que você clicar aumenta seus ganhos. O valor da aposta é multiplicado pelo número de minas restantes.
- **Perder saldo**: Se você clicar em uma célula com bomba, você perde o valor apostado.
- **Parar o jogo a qualquer momento**: O jogador pode parar antes de clicar em uma bomba.
- **Iniciar o jogo**: Começar uma nova rodada de apostas, onde as células são geradas aleatoriamente com e sem bombas.

## Estrutura Técnica

Este projeto foi desenvolvido usando Vue.js e Vuetify para criar uma interface de usuário responsiva e agradável.

### Funcionalidades Técnicas

1. **Estrutura de Dados da Grade (Grid)**
   - A grade é uma matriz de 5x5, representando as células do jogo. Cada célula é representada por um objeto que possui as propriedades `hasBomb` (se a célula contém uma bomba) e `revealed` (se a célula foi revelada após o clique).
   - A função `generateGrid` é responsável por preencher a matriz de células e distribuir as bombas de forma aleatória.

2. **Lógica do Jogo**
   - Quando o jogador clica em uma célula, a função `handleClick` é chamada. Se a célula contiver uma bomba (`cell.hasBomb`), o saldo é descontado, e o jogo é reiniciado. Caso contrário, o valor apostado é multiplicado pelo número de bombas restantes, e os ganhos do jogador são atualizados.
   - A multiplicação dos ganhos é feita com base na seguinte fórmula: `this.ganhos += this.currentBet * this.bombsCount;`.
   - O método `startGame` inicia o jogo, habilitando os botões de ação e gerando uma nova grade. O botão de "parar" só fica habilitado após o início do jogo, permitindo que o jogador interrompa o jogo antes de clicar em uma bomba.

3. **Interação com o Usuário**
   - A interface de usuário é feita com Vuetify, utilizando componentes como `v-btn` para botões e `v-text-field` para capturar valores de entrada.
   - O jogo oferece feedback visual em tempo real com cores diferenciadas para células reveladas, onde as células seguras são verdes e as que contêm bombas são vermelhas. O ícone correspondente também é exibido, com `mdi-bomb` para as células com bomba e `mdi-check-circle` para as células seguras.

4. **Diálogos**
   - O jogo utiliza o `v-dialog` do Vuetify para mostrar feedback visual quando o jogador encontra uma bomba ou se o saldo for insuficiente.

5. **Responsividade**
   - A interface foi projetada para ser responsiva, com layouts ajustáveis para telas menores (como smartphones) e maiores (como desktops). A grade de botões é ajustada automaticamente para diferentes tamanhos de tela, garantindo uma experiência de usuário otimizada em qualquer dispositivo.

## Tecnologias Utilizadas

- **Vue.js**: Framework JavaScript para a construção da interface reativa.
- **Vuetify**: Biblioteca de componentes de interface de usuário baseada no Material Design.
- **CSS**: Estilos para responsividade e personalização visual.

## Considerações Finais

Este projeto foi desenvolvido com o intuito de testar e demonstrar as minhas habilidades em desenvolvimento front-end, especialmente com Vue.js e Vuetify. Embora o jogo seja simples, ele ilustra conceitos importantes como manipulação de estado, componentes dinâmicos, lógica de jogo interativa e design responsivo.

Este projeto é apenas uma demonstração de conhecimento e não está destinado a ser uma aplicação final para produção. No futuro, poderia ser expandido com funcionalidades mais avançadas e melhorias de usabilidade.

---
