# Proposta de Desafio

## 💸 App de Organização de Finanças Pessoais com Vibe Coding

Aprenda a **criar soluções com IA** de forma criativa, guiando ferramentas como o **Copilot** e o **Lovable** com uma comunicação simples e natural. O foco é desenvolver o conceito de um **App de Organização de Finanças Pessoais**, mas, acima de tudo, aprender o **jeito Vibe de programar com IA**.

## ✨ O que é Vibe Coding

**Vibe Coding** é uma forma leve e criativa de desenvolver com IA, baseada em **conversas naturais e bem estruturadas**. Você não precisa escrever código linha por linha. Em vez disso, aprende a **guiar a IA** descrevendo suas ideias de forma clara, com **intenção e contexto**. Em outras palavras:

> Você mostra a vibe da sua ideia e a IA transforma em solução (ou em um caminho para ela).

## 🎯 Desafio

Problema: Muitas pessoas não conseguem manter um controle financeiro porque os aplicativos exigem muita entrada de dados manual, e a criação de orçamentos é vista como algo tedioso. 

Precisamos de uma solução que permita **controlar as finanças por meio de uma conversa simples**, com **agentes de IA** capazes de criar **planos de economia personalizados e automatizados**. Você deve utilizar as ideias de **Vibe Coding** e **MVP (Produto Mínimo Viável)** para desenvolver o **conceito de um aplicativo** que resolva o problema citado.


# 🪄 Etapas da Entrega Desafio

## App de Finanças: Grana Clara

Resultado: https://granaclaraweb.lovable.app

### 1. PRD e interações

```
# Contexto
Quero criar um aplicativo de Organização de Finanças Pessoais que funcione por meio de conversas com o usuário (com linguagem natural).
A ideia é facilitar o controle financeiro de forma simples e natural, sem formulários manuais ou planilhas complexas.

## Problema
Muitas pessoas desistem de controlar seus gastos porque os apps atuais exigem muita entrada manual e pouca personalização.
Quero resolver isso com uma experiência de conversa e recomendações automáticas de economia.

## Público-Alvo
Pessoas que querem começar a organizar suas finanças de forma prática e sem complicação, principalmente iniciantes.

## Moeda e Escopo Geográfico
- MVP suporta apenas **Real (BRL)**.
- **Fora de escopo no MVP:** integração bancária, Open Finance e PIX automático. Todo registro deve ser manual via chat.

---

# Escopo do MVP (Faseado)

Para garantir um MVP robusto (e não raso em várias frentes ao mesmo tempo), o desenvolvimento é dividido em fases. **A IA de código deve implementar apenas a Fase 1 a menos que instruído explicitamente a avançar.**

### Fase 1 — MVP Core
1. Registrar gastos via chat em linguagem natural.
2. Classificar automaticamente as transações, com confirmação/edição do usuário antes de salvar.
3. Visualizar relatório simples (gastos por categoria, por período).

### Fase 2
4. Definir e acompanhar metas financeiras.

### Fase 3
5. Receber dicas de economia do "Agente Financeiro" (recomendações proativas baseadas no histórico).

---

# Funcionalidades-Chave (detalhadas)

## 1. Registro de gastos via chat
- Usuário digita algo como "gastei 50 reais no mercado ontem".
- Sistema extrai: valor, categoria sugerida, data (relativa ou absoluta) e descrição.
- **Toda transação extraída deve ser exibida em um card de confirmação** (valor, categoria, data editáveis) antes de ser persistida no banco. Nunca salvar automaticamente sem confirmação visual.

## 2. Classificação automática
- Categorias iniciais sugeridas: Alimentação, Transporte, Moradia, Saúde, Lazer, Educação, Compras, Outros.
- Quando a confiança da classificação for baixa, o sistema deve perguntar ao usuário em vez de adivinhar (ex: "Isso foi Saúde ou Cuidados Pessoais?").
- Usuário pode reclassificar manualmente a qualquer momento; reclassificações devem alimentar melhoria futura da categorização (aprendizado simples por usuário, ex: regras de palavras-chave já usadas antes).

## 3. Metas financeiras (Fase 2)
- Meta simples: valor-alvo + categoria ou geral + prazo.
- Acompanhamento visual de progresso (barra ou percentual).

## 4. Agente Financeiro (Fase 3)
- Gera dicas com base em padrões de gasto (ex: "Você gastou 30% a mais em Lazer este mês").
- **Guardrail crítico:** o Agente Financeiro NUNCA deve sugerir investimentos, produtos financeiros ou qualquer recomendação que se pareça com consultoria financeira regulada. Escopo estritamente limitado a organização, categorização e economia de gastos do dia a dia.

## 5. Relatórios
- Visualização simples: total por categoria, comparação mês a mês, evolução de saldo.
- Sem gráficos complexos ou customização avançada no MVP — foco em clareza para leigos.

---

# Segurança e Privacidade (LGPD)

Dados financeiros são sensíveis. O MVP deve implementar, desde o início:

- **Autenticação de usuário** obrigatória (ex: Supabase Auth com e-mail/senha ou magic link) — nenhuma funcionalidade financeira acessível sem login.
- **Isolamento de dados por usuário** via Row Level Security (RLS) no banco, garantindo que um usuário nunca acesse dados de outro.
- **Conformidade com a LGPD**: base legal para tratamento de dados financeiros, política de privacidade acessível, e opção de exclusão de conta com apagamento de dados.
- Nenhum dado sensível (senhas, tokens, chaves de API) deve estar hardcoded no código — apenas em variáveis de ambiente (`.env`), conforme já definido nos Code Patterns.
- Comunicação sempre via HTTPS; sem armazenamento de dados financeiros em logs de aplicação.

---

# Guidelines

## Code Patterns
- Constantes sempre escritas em SCREAMING_CASE, variáveis em camelCase e nomes de arquivo em kebab-case.
- Evitar números mágicos.
- Separar responsabilidades de forma apropriada (ex: lógica de parsing de linguagem natural separada da lógica de persistência).
- Usar senhas e chaves apenas no arquivo `.env`, nunca hardcoded.
- Validar todo valor monetário recebido: não aceitar valores negativos, símbolos inconsistentes ou formatos inválidos antes de persistir.

## Database Patterns
- Sempre usar padrão UUID como chave primária.
- Tabelas com prefixo e nomes no plural.
- Modelo de dados mínimo esperado no MVP:
  - `users` (dados de conta e autenticação)
  - `transactions` (valor, categoria, descrição, data, usuário, origem: chat/manual)
  - `categories` (nome, ícone, tipo — padrão do sistema ou customizada pelo usuário)
  - `goals` (Fase 2: valor-alvo, categoria, prazo, usuário)

## API Patterns
- Endpoints sempre versionados (ex: `/api/v1/...`) e todos no plural (ex: `/transactions`, não `/transaction`).
- Rotas separadas e organizadas conforme responsabilidade (ex: rotas de transações, categorias, metas e chat/IA isoladas em módulos distintos).
- Respostas de erro em formato padronizado e consistente (ex: `{ error: { code, message } }`).
- Autenticação via token (ex: JWT) em todas as rotas que exigem usuário logado.

## Design Patterns
- Design universal para melhor UI/UX possível para todos os públicos, principalmente para leigos.
- Seguir diretrizes de acessibilidade WCAG AA como referência: contraste adequado, tamanho de fonte legível, área de toque mínima para elementos interativos.
- Usabilidade simples sem sobrecarga de informações — priorizar poucas ações por tela.
- Layout responsivo bem aplicado para todos os tamanhos de tela, com prioridade em mobile-first (uso principal esperado via celular).

---

# Telas Mínimas do MVP (Fase 1)

1. **Onboarding** — criação de conta / login.
2. **Chat principal** — interface de conversa para registro de gastos.
3. **Card de confirmação de transação** — revisão e edição antes de salvar (valor, categoria, data).
4. **Dashboard / Relatório** — visão geral de gastos por categoria e por período.
5. **Histórico de transações** — lista simples com filtro por categoria/data.

*(Telas de Metas e Agente Financeiro entram nas Fases 2 e 3, respectivamente.)*

---

# Guardrails

- Antes de gerar novo código, sempre conferir se faz sentido com o contexto e se não irá comprometer as funcionalidades principais.
- Implementar apenas o escopo da Fase 1 a menos que explicitamente instruído a avançar de fase.
- Nenhuma transação deve ser persistida no banco sem confirmação visual do usuário.
- O Agente Financeiro nunca deve fornecer recomendações de investimento ou consultoria financeira regulada.
- Toda funcionalidade que envolva dados financeiros exige usuário autenticado e isolamento de dados (RLS).
- Chamadas a serviços de IA/NLP devem ter tratamento de erro e fallback (ex: se a IA não conseguir extrair uma transação, pedir ao usuário para reformular, nunca falhar silenciosamente).

---

# Entregável da IA
Gerar um plano de MVP (Fase 1) com as principais telas, recursos necessários e um esboço de validação inicial.
Usar tom educativo e linguagem acessível, em português.

```

PRD construído e depois revisado usando Claude Code para correções e sugestões.

Posteriormente foi usado um prompt para refinamento e correções de funcionalidades e layout. Também para ajustes em quesitos de segurança.

```
## Realize as seguintes melhorias:
- Nome do app deve aparecer no header e footer.
- Opção para alternar entre modo noturno/claro.
- O chat também deve incluir dinheiro de entrada a ser contabilizado.
- Nas telas de chat e histórico, o valor de saldo atual deve ser mostrado constantemente na parte superior com atualização em tempo real.
- Incluir login de teste.
```

```
Atue como um especialista em cibersegurança e revise este código para garantir a implementação de boas práticas de segurança. Valide o gerenciamento de estado e a autenticação para que sejam seguros, mantendo a fluidez e a funcionalidade intactas.
```

### 2. Telas do app

<img width="1366" height="736" alt="image" src="https://github.com/user-attachments/assets/5d66437d-4608-449b-9afe-e460e9905159" />

<img width="1366" height="736" alt="image" src="https://github.com/user-attachments/assets/ec497ae8-3a11-4d85-b210-cad51dae7a24" />

<img width="1366" height="736" alt="image" src="https://github.com/user-attachments/assets/9b96f245-0f62-44db-9377-e088091fbe09" />

<img width="1366" height="736" alt="image" src="https://github.com/user-attachments/assets/47f32bdd-244c-4d9c-b3b7-1bb55a9b2e05" />

### 3. Funcionalidades

- Registrar entradas e saídas diretamente via chat, onde o app identifica o tipo de gasto e o categoriza, sendo registrado apenas após confirmação do usuário.
- Em caso de erro ou falha, o chat nunca reage silenciosamente.
- Gerado relatórios para se ter ideia em que atividade e categorias que mais geram gastos.
- Registro de saldo atual com atualizações em tempo real.
- Exibição de histórico de registros com possibilidade de deleção dos mesmos.

### 4. Reflexões

- O que funcionou bem?
  Uso do PRD para impor especificações para o desenvolvimento, servindo como um excelente fundamento de bases para IA desenvolver.
- O que não funcionou como esperado?
  IA acaba se perdendo em alguns pequenos detalhes e aspectos, o que reforça a presença de um olhar analítico e criterioso para impor especificações.
- O que foi aprendido?
  O grande poder do uso de IAs para desenvolvimento usando PRD com specs bem definidas e elaboradas.

## 💬 Conclusão

Vibe Coding é sobre clareza, curiosidade e criatividade, não sobre perfeição técnica. O verdadeiro objetivo aqui é aprender a pensar junto com a IA, transformando ideias em conceitos reais e enxergando a tecnologia como uma extensão do seu raciocínio criativo. Cada interação é um experimento, quanto mais clara for sua intenção, mais surpreendente será o resultado.
