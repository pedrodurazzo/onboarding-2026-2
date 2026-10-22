# Relatório de Mapeamento, Reconhecimento e Exploração do Alvo

**Trilha:** Red Team — Bloco 2 (Semanas 3 e 4)  
**Objetivo:** Mapear a superfície de ataque, identificar rotas e endpoints e rastrear o e-mail da conta administradora do sistema.  
**Autor:** Pedro Saunders Alves Durazzo  

---

## 1. Visão Geral do Alvo
Esse relatório documenta a fase de reconhecimento realizada sobre o site **OWASP Juice Shop**. O foco desta etapa consistiu em analisar a estrutura da aplicação além da interface gráfica convencional, interceptando tráfego e inspecionando arquivos *client-side* para identificar dados expostos e contas de privilégio elevado.

---

## 2. Ferramental Utilizado
- **Proxy de Interceptação (Burp Suite)**

---

## 3. Rotas, Arquivos e Endpoints Descobertos

Através da navegação na aplicação combinada com a análise de tráfego no proxy de interceptação, foram mapeadas as seguintes rotas e recursos de interesse no alvo:

### 3.1. Rotas
- `/#/score-board`: Painel de pontuação e desafios do Juice Shop.
- `/#/administration`: Painel administrativo restrito para gerenciamento de usuários e solicitações.
- `/#/privacy-security/privacy-policy`: Rota contendo informações e documentos de políticas de privacidade.

### 3.2. Endpoints e Arquivos Expostos
- `/api/Users`: Endpoint da API responsável por listar/cadastrar usuários.
- `/api/Feedbacks`: Endpoint público de comentários e avaliações contendo dados de usuários.
- `/ftp`: Arquivos exposto.
- `/rest/memories`: Endpoint do Photo Wall que contém dados de um usuário admin.

> **Rotas**
<img width="1900" height="1027" alt="image" src="https://github.com/user-attachments/assets/366848c7-54f6-4d3a-92ef-ff1dcaf7cb5d" />
<img width="1898" height="1031" alt="image" src="https://github.com/user-attachments/assets/47cd574a-467d-4c72-8a34-ae5dde36dbf9" />
<img width="1916" height="321" alt="image" src="https://github.com/user-attachments/assets/7786569a-1d2d-449a-8071-055bf3239a8d" />

> **Endpoints e Arquivos Expostos**
<img width="1919" height="399" alt="ftp" src="https://github.com/user-attachments/assets/aecedc15-3de6-40e6-bb1e-8a5fd31e7778" />
<img width="1919" height="271" alt="endpoints" src="https://github.com/user-attachments/assets/1825ac7f-f92e-4473-95e7-6925d53b35c4" />
<img width="1919" height="220" alt="endpoints2" src="https://github.com/user-attachments/assets/aa9726e0-cad4-4391-a0d7-cbfb54d4c629" />
<img width="1917" height="1030" alt="UsersInfoLeaks" src="https://github.com/user-attachments/assets/c07b6478-c6a9-4aaf-a8ac-baebc34fb6a8" />
<img width="1917" height="1030" alt="BasketChangeQuantity" src="https://github.com/user-attachments/assets/334697b0-2beb-44e7-b6a2-88b1a6c8ce39" />

---

## 4. Identificação do E-mail do Administrador

Para identificar o endereço de e-mail da conta com privilégios de administrador do sistema, seguiu-se o procedimento abaixo:

1. **Análise de Requisições de API:**
   - Durante a navegação inicial e consulta a avaliações de produtos (feedbacks), interceptou-se a requisição para `/api/Feedbacks` ou `/api/Products/1/reviews`.
2. **Inspeção da Estrutura de Usuários:**
   - A resposta JSON contida nos endpoints retornou os objetos associados aos criadores dos comentários, identificando que o `UserId: 1` corresponde à conta admin.
3. **Mapeamento do E-mail da Conta Admin:**
   - A requisição para os detalhes da conta ID 1 ou a verificação dos dados de avaliação revelou o e-mail da conta administradora do sistema:  
     `admin@juice-sh.op`

<img width="1546" height="911" alt="image" src="https://github.com/user-attachments/assets/550cb0b1-b488-4bf5-b546-6f7db9b60f34" />
<img width="1547" height="912" alt="image" src="https://github.com/user-attachments/assets/74a6fa9d-b297-4302-9c5e-12c146e71f59" />
