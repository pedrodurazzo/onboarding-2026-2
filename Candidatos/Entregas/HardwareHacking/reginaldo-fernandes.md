# Onboarding SACI 2026.2 - Hardware Hacking

## Dados do Candidato
* **Nome:** Reginaldo Oliveira Fernandes
* **Curso:** Sistemas de Informação
* **Trilha:** Hardware Hacking

---

## Desafio Técnico Inicial (RFID vs NFC)

### Pergunta:
No contexto de segurança e auditoria física, qual é a principal diferença de frequência e operação entre o **RFID de Baixa Frequência (LF - 125 kHz)**, o **RFID de Alta Frequência (HF - 13.56 MHz)** e o **NFC**? 
Como essa diferença afeta o alcance do ataque e o tipo de cartão que conseguimos clonar?

### Resposta:
A diferença central está na **banda de frequência** e, como consequência direta, na
física de acoplamento e no nível de segurança das tags de cada faixa.

**RFID LF (125 kHz):** usa acoplamento indutivo (campo magnético, campo próximo),
com alcance prático de poucos centímetros. Tem boa penetração em água, tecido
orgânico e proximidade de metal — por isso é usado em chip de animal e
imobilizador de carro. Tags comuns (EM4100, HID Prox) **não possuem autenticação
nenhuma**: elas apenas transmitem o UID para qualquer leitor que gere o campo.
Na prática, são as mais fáceis de clonar — basta ler o UID e regravar numa tag
T5577 com um Proxmark3 ou Flipper Zero.

**RFID HF (13,56 MHz):** também é acoplamento indutivo e campo próximo, mas opera
sob padrões que preveem anticolisão e criptografia (ISO/IEC 14443, ISO/IEC 15693).
O alcance é semelhante, na casa dos centímetros. Aqui a clonagem **depende do chip**:
o Mifare Classic usa a cifra proprietária Crypto1 (chave de 48 bits), quebrada em
2008, e é clonável em minutos via ataques como nested e hardnested; já o Mifare
DESFire EV2/EV3 usa AES-128 com autenticação mútua por desafio-resposta, o que
inviabiliza a clonagem direta.

**NFC:** não é uma frequência à parte — é um **subconjunto especializado do RFID HF**,
operando nos mesmos 13,56 MHz e compatível com ISO 14443 e FeliCa. O que ele
acrescenta não é física nova, e sim **modos de operação**: o mesmo chip pode agir
como leitor, emular um cartão (card emulation, base do pagamento por aproximação)
ou operar ponto a ponto.

**Impacto no alcance do ataque:** em ambas as faixas o campo magnético decai com
1/r³, muito mais rápido que os 1/r² de uma onda propagada. Isso limita naturalmente
a leitura clandestina a centímetros — mas o limite é de *energia*, não de princípio:
com antenas maiores e amplificação é possível estender a leitura para algumas
dezenas de centímetros. Além disso, o **relay attack** contorna a distância por
completo: retransmite as mensagens em tempo real entre dois dispositivos, sem
precisar quebrar criptografia alguma.

**Conclusão para auditoria física:** o fator determinante não é a frequência em si,
mas o **modelo de chip** e se ele implementa autenticação. Um crachá LF 125 kHz ou
um Mifare Classic devem ser tratados como comprometidos por padrão.