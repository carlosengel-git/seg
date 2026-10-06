#include <AFMotor.h>

// Instancia os dois motores nas saídas M1 e M2 do Shield
AF_DCMotor motorEsq(1); 
AF_DCMotor motorDir(2); 

// Definicao dos pinos dos sensores HW-870
const int SENSOR_ESQ = A0;
const int SENSOR_DIR = A1;

// Ajuste a velocidade geral (0 a 255)
const int VELOCIDADE = 160; 

void setup() {
  // Configura os pinos dos sensores como entrada
  pinMode(SENSOR_ESQ, INPUT);
  pinMode(SENSOR_DIR, INPUT);

  // Define a velocidade inicial dos motores
  motorEsq.setSpeed(VELOCIDADE);
  motorDir.setSpeed(VELOCIDADE);

  // Inicia com os motores parados
  parar();
}

void loop() {
  // Leitura digital dos sensores HW-870
  // Lógica padrão: HIGH = Fundo claro | LOW = Linha preta
  int leituraEsq = digitalRead(SENSOR_ESQ);
  int leituraDir = digitalRead(SENSOR_DIR);

  // 1. Ambos no fundo claro -> Segue reto
  if (leituraEsq == HIGH && leituraDir == HIGH) {
    frente();
  }
  // 2. Sensor Esquerdo na linha preta -> Virar para a Esquerda
  else if (leituraEsq == LOW && leituraDir == HIGH) {
    virarEsquerda();
  }
  // 3. Sensor Direito na linha preta -> Virar para a Direita
  else if (leituraEsq == HIGH && leituraDir == LOW) {
    virarDireita();
  }
  // 4. Ambos na linha preta (fresta/cruzamento) -> Parar
  else if (leituraEsq == LOW && leituraDir == LOW) {
    parar();
  }
}

// --- Funcoes de Movimentacao ---

void frente() {
  motorEsq.run(FORWARD);
  motorDir.run(FORWARD);
}

void virarEsquerda() {
  motorEsq.run(RELEASE); // Para a roda esquerda
  motorDir.run(FORWARD); // Mantem a roda direita girando
}

void virarDireita() {
  motorEsq.run(FORWARD); // Mantem a roda esquerda girando
  motorDir.run(RELEASE); // Para a roda direita
}

void parar() {
  motorEsq.run(RELEASE);
  motorDir.run(RELEASE);
}
