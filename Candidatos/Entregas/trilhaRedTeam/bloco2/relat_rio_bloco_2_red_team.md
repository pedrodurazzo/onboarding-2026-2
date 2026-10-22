# Relatório de Mapeamento, Reconhecimento e Exploração do Alvo

**Trilha:** Red Team — Bloco 2 (Semanas 3 e 4)  
**Alvo:** OWASP Juice Shop  
**Objetivo:** Mapear a superfície de ataque, identificar rotas/endpoints ocultos e rastrear o e-mail da conta administradora do sistema.  
**Data:** [Data da Execução]  
**Autor:** [Seu Nome / ID]  

---

## 1. Visão Geral do Alvo
O presente relatório documenta a fase de reconhecimento passivo e ativo realizada sobre a aplicação web **OWASP Juice Shop**. O foco desta etapa consistiu em analisar a estrutura da aplicação além da interface gráfica convencional, interceptando tráfego HTTP/HTTPS e inspecionando arquivos do lado do cliente (*client-side*) para identificar ativos expostos e contas de privilégio elevado.

---

## 2. Ferramental Utilizado
- **Navegador Web & DevTools (F12):** Utilizado para análise de arquivos estáticos (`main.js`, `runtime.js`), inspeção da árvore DOM, acompanhamento de requisições via aba *Network* e análise do *Console*.
- **Proxy de Interceptação (Burp Suite / OWASP ZAP):** Mapeamento automatizado do mapa do site (*Site Map*), interceptação de requisições/respostas HTTP e análise de endpoints da API REST.

---

## 3. Rotas, Arquivos e Endpoints Descobertos

Através da navegação na aplicação combinada com a análise de tráfego no proxy de interceptação, foram mapeadas as seguintes rotas e recursos de interesse no alvo:

### 3.1. Rotas do Frontend (Angular Application)
- `/#/score-board`: Painel de pontuação e desafios do Juice Shop.
- `/#/administration`: Painel administrativo restrito para gerenciamento de usuários e solicitações.
- `/#/privacy-security/privacy-policy`: Rota contendo informações e documentos de políticas de privacidade.

### 3.2. Endpoints de API REST & Arquivos Expostos
- `/api/Users`: Endpoint da API responsável por listar/cadastrar usuários.
- `/api/Feedbacks`: Endpoint público de comentários e avaliações contendo metadados de usuários.
- `/assets/public/`: Diretório contendo arquivos públicos e recursos estáticos da aplicação.
- `/ftp`: Diretório de arquivos exposto com downloads disponíveis (ex.: logs, cópias de segurança e arquivos `.bak`).

> **[INSERIR PRINT 1 AQUI]**  
> *Comprovação das rotas, arquivos ou endpoints descobertos via Burp Suite ou DevTools (Network/Site Map).*

---

## 4. Metodologia de Identificação do E-mail do Administrador

Para identificar o endereço de e-mail da conta com privilégios de administrador do sistema, seguiu-se o procedimento abaixo:

1. **Análise de Requisições de API:**
   - Durante a navegação inicial e consulta a avaliações de produtos (feedbacks), interceptou-se a requisição para `/api/Feedbacks` ou `/api/Products/1/reviews`.
2. **Inspeção da Estrutura de Usuários:**
   - A resposta JSON contida nos endpoints retornou os objetos associados aos criadores dos comentários, identificando que o `UserId: 1` corresponde à conta raiz/administradora.
3. **Mapeamento do E-mail da Conta Admin:**
   - A requisição para os detalhes da conta ID 1 ou a verificação dos dados de avaliação revelou o e-mail da conta administradora do sistema:  
     `admin@juice-sh.op`

> **[INSERIR PRINT 2 AQUI]**  
> *Comprovação da requisição/resposta HTTP ou inspeção no navegador onde o e-mail `admin@juice-sh.op` é identificado.*

---

## 5. Evidências e Capturas de Tela

### Evidência 01 — Mapeamento de Rotas e Endpoints Ocultos
> **[INSERIR PRINT AQUI]**  
> *Legenda: Print demonstrando o mapa de rotas descobertas (ex: Burp Suite Site Map, aba Network do F12 ou acesso direto a endpoints como `/ftp` ou `/#/score-board`).*

### Evidência 02 — Rastreamento do E-mail da Conta Administradora
> **[INSERIR PRINT AQUI]**  
> *Legenda: Print mostrando o e-mail `admin@juice-sh.op` capturado em uma resposta de API REST ou painel de dados.*

---

## 6. Conclusões e Considerações de Segurança
- **Exposição Involuntária de Dados:** A aplicação expõe informações sensíveis de estrutura de usuários através de retornos da API REST sem a devida sanitização ou autenticação.
- **Visibilidade de Rotas Administrativas:** Rotas restritas e diretórios sensíveis (como `/ftp`) estão visíveis no mapa do site e arquivos estáticos.
- **Recomendação:** Implementar controles rígidos de autorização em nível de API (RBAC) e restringir a navegação direta em diretórios estáticos e rotas internas em ambiente de produção.