# Relatório de Mapeamento, Reconhecimento e Exploração do Alvo

**Trilha:** Red Team  
**Objetivo:** Mapear a superfície de ataque da aplicação, investigar recursos ocultos e coletar informações estratégicas sobre o sistema e seus usuários.
**Nome:** Pedro Saunders Alves Durazzo  

---

## 1. Visão Geral
Esse relatório documenta a fase de reconhecimento do site **OWASP Juice Shop**. Esta etapa é analisar a estrutura além da página inicial, interceptando tráfego e inspecionando arquivos side-client para identificar dados expostos e contas admins.

---

## 2. Ferramental Utilizado
**Proxy de Interceptação (Burp Suite)**

---

## 3. Rotas, Arquivos e Endpoints Descobertos

### 3.1. Rotas
- `/#/score-board`: Painel de pontuação e desafios do site.
- `/#/administration`: Painel de admin.
- `/#/privacy-security/privacy-policy`: Informações de políticas de privacidade.

**Rotas:**
<img width="1900" height="1027" alt="image" src="https://github.com/user-attachments/assets/366848c7-54f6-4d3a-92ef-ff1dcaf7cb5d" />
<img width="1898" height="1031" alt="image" src="https://github.com/user-attachments/assets/47cd574a-467d-4c72-8a34-ae5dde36dbf9" />
<img width="1916" height="321" alt="image" src="https://github.com/user-attachments/assets/7786569a-1d2d-449a-8071-055bf3239a8d" />

---

### 3.2. Endpoints e Arquivos Expostos
- `/api/Users`: Endpoint da API responsável por listar/cadastrar usuários.
- `/api/Feedbacks`: Endpoint público de comentários e avaliações contendo dados de usuários. E também permite forjar Feedbacks de outros usuários utilizado o User ID.
- `/ftp`: Arquivos exposto.
- `/rest/memories`: Endpoint do Photo Wall que contém dados de um usuário admin.
- `/api/BasketsItens`: Endpoint do carrinho que permite alterar a quantidade até para números negativos.
- `/rest/basket/1`: Endpoint do carrinho de compras que é possível acessar o carrinho de outros usuários.

**Endpoints e Arquivos Expostos:**
<img width="1919" height="399" alt="ftp" src="https://github.com/user-attachments/assets/aecedc15-3de6-40e6-bb1e-8a5fd31e7778" />
<img width="1919" height="271" alt="endpoints" src="https://github.com/user-attachments/assets/1825ac7f-f92e-4473-95e7-6925d53b35c4" />
<img width="1919" height="220" alt="endpoints2" src="https://github.com/user-attachments/assets/aa9726e0-cad4-4391-a0d7-cbfb54d4c629" />
<img width="1917" height="1030" alt="UsersInfoLeaks" src="https://github.com/user-attachments/assets/c07b6478-c6a9-4aaf-a8ac-baebc34fb6a8" />
<img width="1917" height="1030" alt="BasketChangeQuantity" src="https://github.com/user-attachments/assets/334697b0-2beb-44e7-b6a2-88b1a6c8ce39" />
<img width="1917" height="1030" alt="usersbaskets" src="https://github.com/user-attachments/assets/7409ac12-2925-4406-be39-31730f76fadc" />
<img width="1917" height="1030" alt="ForgedFeedbacks" src="https://github.com/user-attachments/assets/3771ec53-2e59-454b-ab6c-d70aa80abac9" />

---

## 4. Identificação do E-mail do Administrador

1. **Análise de Requisições de API:**
   - Durante a navegação inicial e consulta a avaliações de produtos, interceptou-se a requisição para `/api/Feedbacks` ou `/rest/products/1/reviews`.
2. **Inspeção da Estrutura de Usuários:**
   - Os dado nos endpoints retornou os objetos associados aos criadores dos comentários, identificando que o `UserId: 1` corresponde a uma conta admin.
3. **Identificando o E-mail da Conta Admin:**
   - A requisição para os detalhes da conta ID 1 ou a verificação dos dados de avaliação mostra o e-mail da conta admin do sistema:  
     `admin@juice-sh.op`

<img width="1546" height="911" alt="image" src="https://github.com/user-attachments/assets/550cb0b1-b488-4bf5-b546-6f7db9b60f34" />
<img width="1547" height="912" alt="image" src="https://github.com/user-attachments/assets/74a6fa9d-b297-4302-9c5e-12c146e71f59" />
<img width="1549" height="909" alt="image" src="https://github.com/user-attachments/assets/810f5761-1baa-4520-8a71-356dbda47a33" />

