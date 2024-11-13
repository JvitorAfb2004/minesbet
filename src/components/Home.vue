<template>
    <v-container class="mt-10">
      <!-- Caixa de Informações e Grid -->
      <v-row>
        <!-- Coluna da Caixa de Informações -->
        <v-col cols="12" md="4" style="border:1px solid #f1f1f1; border-radius: 10px;">
          <v-card class="pa-4">
            <v-row>
              <v-col>
                <v-card-title class="headline">Meu Saldo</v-card-title>
                <v-card-subtitle class="subheading">R$ {{ saldo }}</v-card-subtitle>
              </v-col>
              <v-col>
                <v-card-title class="headline">Ganhos</v-card-title>
                <v-card-subtitle class="subheading">R$ {{ ganhos }}</v-card-subtitle>
              </v-col>
            </v-row>
          </v-card>
  
          <v-card class="pa-4">
            <v-card-title class="headline">Quantia Apostada</v-card-title>
            <v-text-field 
              variant="outlined"
              type="number"
              prefix="R$"
              v-model="quantia"
              :disabled="gameStarted"
            ></v-text-field>
          </v-card>
  
          <v-card class="pa-4">
            <v-card-title class="headline">Número de Minas</v-card-title>
            <v-text-field 
              variant="outlined"
              type="number"
              v-model="bombsCount"
              :disabled="gameStarted"
            ></v-text-field>
          </v-card>
  
          <v-btn 
            :disabled="gameStarted" 
            @click="startGame" 
            color="primary"
            class="ma-2"
            style="width: 150px !important;">
            Começar
          </v-btn>
  
          <v-btn 
            :disabled="!podeParar" 
            @click="stopGame" 
            color="secondary" 
            class="ma-2"
            style="width: 150px !important;">
            Parar
          </v-btn>
        </v-col>
  
        <!-- Coluna do Grid -->
        <v-col cols="12" md="8">
          <!-- Contêiner para a grade -->
          <v-row justify="center">
            <v-col cols="6" md="8">
              <v-row v-for="(row, rowIndex) in chunkedGrid" :key="rowIndex">
                <v-col v-for="(cell, cellIndex) in row" :key="cellIndex" cols="2">
                  <v-btn
                    :class="{
                      'green': cell.revealed && !cell.hasBomb,
                      'red': cell.revealed && cell.hasBomb,
                    }"
                    :disabled="cell.revealed || !gameStarted"
                    @click="handleClick(cell, rowIndex, cellIndex)"
                  >
                    <v-icon v-if="cell.revealed">
                      {{ cell.hasBomb ? 'mdi-bomb' : 'mdi-check-circle' }}
                    </v-icon>
                  </v-btn>
                </v-col>
              </v-row>
            </v-col>
          </v-row>
        </v-col>
      </v-row>
  
      <!-- Diálogo de Bomba -->
      <v-dialog v-model="dialog" width="auto">
        <v-card max-width="400" prepend-icon="mdi-bomb" color="red" title="Você encontrou uma bomba">
        </v-card>
      </v-dialog>
  
      <!--Diálogo saldo negativo-->
      <v-dialog v-model="dialogSaldoNegativo" width="auto">
        <v-card max-width="400" prepend-icon="mdi-cash" color="red" title="Saldo Insuficiente!">
        </v-card>
      </v-dialog>
    </v-container>
  </template>
  
  <script>
  export default {
    data() {
      return {
        saldo: 100,        // Exemplo de saldo
        dialogSaldoNegativo:false,
        quantia: 50,       // Valor apostado
        ganhos: 0,
        grid: [],          // Matriz 5x5 de células
        size: 5,           // Tamanho da grade (5x5)
        bombsCount: 5,     // Número de bombas
        gameStarted: false, // Flag para indicar se o jogo começou
        dialog: false,      // Controle do diálogo
        currentBet: 0,      // Valor da aposta atual
        podeParar: false
      };
    },
    computed: {
      // Quebra a grade em linhas de 5 células para exibição
      chunkedGrid() {
        const rows = [];
        for (let i = 0; i < this.grid.length; i += this.size) {
          rows.push(this.grid.slice(i, i + this.size));
        }
        return rows;
      },
    },
    methods: {
      // Função para gerar a grade e distribuir as bombas
      generateGrid() {
        const totalCells = this.size * this.size;
        const bombPositions = new Set();
  
        // Distribui as bombas aleatoriamente
        while (bombPositions.size < this.bombsCount) {
          const randomPosition = Math.floor(Math.random() * totalCells);
          bombPositions.add(randomPosition);
        }
  
        // Preenche a grade com as bombas e células vazias
        this.grid = Array.from({ length: totalCells }, (_, index) => ({
          hasBomb: bombPositions.has(index),
          revealed: false,
        }));
      },
  
      // Função chamada ao clicar em uma célula
      handleClick(cell, rowIndex, cellIndex) {
        this.podeParar = true
        if (cell.hasBomb) {
          this.grid[rowIndex * this.size + cellIndex].revealed = true;
          this.dialog = true;
          this.ganhos = 0
          this.podeParar = false
  
          setTimeout(() => {
            this.dialog = false;
            this.resetGame();
          }, 1000);
        } else {
          this.grid[rowIndex * this.size + cellIndex].revealed = true;
          // Se a célula não tem bomba, aumenta o saldo pela aposta multiplicada
          this.ganhos += this.currentBet * ("1." + this.bombsCount);

        }
      },
  
      // Função para reiniciar o jogo
      resetGame() {
        this.generateGrid();
        this.gameStarted = false;
      },
  
      // Função para começar o jogo
      startGame() {
      if(this.saldo < this.quantia){
        this.dialogSaldoNegativo = true

      }else{
        this.currentBet = this.quantia;
        this.gameStarted = true;
        this.saldo -= this.quantia
        this.generateGrid(); // Gerar o tabuleiro ao começar
      }
      },
  
      // Função para parar o jogo
      stopGame() {
        this.gameStarted = false;
        this.saldo += this.ganhos
        this.ganhos = 0
      },
    },
    created() {
      this.generateGrid(); // Gerar o tabuleiro ao inicializar
    },
  };
  </script>
  
  <style scoped>
  .v-btn {
    width: 100px !important;
    height: 60px !important;
    font-size: 20px;
  }
  .v-card{
    box-shadow: none !important;
  }
  
  .green {
    background-color: #4caf50 !important;
  }
  
  .red {
    background-color: #f44336 !important;
  }
  .v-icon{
    color:white
  }
  </style>
  