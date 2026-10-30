# Onboarding SACI 2026.2 - Hardware Hacking

## Dados do Candidato
* **Nome:** Emanuel Oliveira de Assis Domingues
* **Curso:** Ciência da Computação
* **Trilha:** Hardware Hacking

---

## Desafio Técnico Inicial (RFID vs NFC)

### Pergunta:
No contexto de segurança e auditoria física, qual é a principal diferença de frequência e operação entre o **RFID de Baixa Frequência (LF - 125 kHz)**, o **RFID de Alta Frequência (HF - 13.56 MHz)** e o **NFC**? 
Como essa diferença afeta o alcance do ataque e o tipo de cartão que conseguimos clonar?

### Resposta:
O RFID LF (125 kHz) tem alcance de 5 a 10 cm e permite a clonagem simples de crachás de acesso (como EM4100 e HID Prox) que não têm criptografia: basta ler o ID e gravá-lo em uma tag regravável (como a T5577). Já o RFID HF opera em 13,56 MHz e tem alcance parecido, de até cerca de 15 cm, mas envolve cartões inteligentes (como o MIFARE), então o atacante precisa ficar próximo do cartão durante o ataque à criptografia. Por fim, o NFC também opera em 13,56 MHz, por ser um subconjunto do HF, com alcance ainda menor, de cerca de 4 cm, e comunicação bidirecional (smartphones e cartões de pagamento). Os cartões de pagamento são mais vulneráveis a ataques de relay do que à clonagem, mas ainda é possível clonar tags e cartões de acesso NFC sem criptografia forte (como o MIFARE Classic). Ou seja, o que possibilita a clonagem é a criptografia do cartão, e não o rótulo da tecnologia (HF ou NFC).