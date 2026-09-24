# 💸 FinChat: Organização Financeira via IA (Vibe Coding)

Este repositório contém o conceito e o Produto Mínimo Viável (MVP) do **FinChat**, um aplicativo de finanças pessoais impulsionado por IA, desenvolvido como parte do desafio de Vibe Coding da DIO. O objetivo principal do projeto é eliminar a fricção do registro manual de gastos através de uma interface 100% conversacional.

## 🚀 Status do Projeto e Testes Realizados
O FinChat já está no ar e foi testado com sucesso! As seguintes funcionalidades estão operacionais no MVP gerado:
* **Autenticação:** Cadastro de usuários funcional (com exigência de confirmação por e-mail para segurança).
* **Interface Responsiva:** O app foi projetado com foco na visão de celular (mobile-first), podendo ser alternado no controle de dispositivos.
* **Chat e Categorização:** Interação fluida com o "Cents". Em testes reais, o comando *"Comprei um jogo por 150"* foi automaticamente interpretado, anotado como R$ 150 na categoria **Lazer**, atualizando o saldo e os resumos de gastos instantaneamente.
* **Recursos Extras:** Painel de resumos interativo e exportação de relatório mensal em PDF totalmente funcionais.

## 🧠 Resumo do Conceito
O FinChat inverte a lógica dos apps de finanças tradicionais. Em vez de navegar por menus complexos e preencher formulários com datas, categorias e valores, o usuário simplesmente "conversa" com o app. Você digita ou envia um áudio dizendo: *"Gastei 45 reais com um lanche no iFood hoje"*, e o Agente Financeiro (IA) interpreta, categoriza (Alimentação/Delivery), registra a data, deduz do orçamento mensal e responde com o saldo atualizado e dicas de economia.

## 🎯 O Prompt Final (PRD Otimizado)

Para guiar o Lovable/Copilot na geração deste MVP, o seguinte PRD foi estruturado focando em clareza, intenção de produto e limites técnicos:

> **# Contexto do Produto**
> Quero criar o "FinChat", um app de Organização de Finanças Pessoais focado em uma interface "Chat-First". A interação deve simular uma conversa no WhatsApp com um consultor financeiro. Nada de planilhas visíveis ou formulários longos.
>
> **# Problema a Resolver**
> A alta taxa de abandono (churn) em apps de finanças ocorre devido ao tédio e à fricção da entrada manual de dados. Os usuários esquecem de anotar ou têm preguiça de categorizar gastos.
>
> **# Público-Alvo**
> Jovens adultos e universitários que buscam controle financeiro rápido, prático e integrado à rotina dinâmica, sem jargões contábeis.
>
> **# Funcionalidades-Chave (MVP)**
> 1. **Input em Linguagem Natural:** Registro de receitas e despesas via chat (ex: "Recebi meu salário de 3000" ou "Comprei um jogo por 150").
> 2. **Auto-Categorização Inteligente:** A IA identifica a categoria do gasto automaticamente e pede confirmação apenas se houver ambiguidade.
> 3. **Dashboard Resumido (Cards):** Ao puxar a tela para baixo, o chat revela cards dinâmicos com Saldo, Gastos do Mês e Meta de Economia.
> 4. **Insights Proativos:** Alertas amigáveis quando o usuário ultrapassa 80% do limite de uma categoria.
> 5. **Exportação Simples:** Capacidade de gerar um relatório mensal em PDF com um único comando de texto.
>
> **# O Agente Financeiro (Persona)**
> O assistente deve se chamar "Cents". Tom de voz: casual, encorajador e direto. Sem sermões. Se o usuário gastar muito, o Cents deve focar em como ajustar o resto do mês em vez de focar na culpa.
>
> **# Entregável Esperado da IA**
> 1. Um mapeamento do fluxo de telas do usuário (User Flow).
> 2. O comportamento detalhado do bot para o cenário de um "gasto por impulso".
> 3. Um plano de validação técnica do MVP (como medir o engajamento inicial).

## 📱 Entregáveis do PRD (Gerados pela IA)

Com base nas diretrizes, a IA gerou os seguintes entregáveis técnicos:

### 1. Mapeamento do fluxo de telas
Sem telas de formulário, listas ou edição: tudo acontece na conversa.

```text
[Entrada]
  /auth  — Criar conta (nome, e-mail, senha) | Entrar | Continuar com Google
     |  sessão ativa
     v
[Tela única: Chat]  /
  ├─ Cabeçalho: avatar do Cents, status "online", sair
  ├─ Botão "puxe para ver seus resumos" (padrão: recolhido)
  │     └─ Painel de resumos
  │          ├─ Card SALDO (entradas − saídas, acumulado)
  │          ├─ Card GASTOS DO MÊS (com orçamento mensal)
  │          ├─ Card META DE ECONOMIA (% atingido)
  │          └─ Barras por categoria (amarelo a partir de 80% do limite)
  ├─ Conversa
  │     ├─ Bolha do usuário (direita)
  │     ├─ Bolha do Cents (esquerda) + cartão de lançamento
  │     ├─ Cartão de alerta proativo (80% do limite)
  │     └─ Confirmação de relatório baixado
  └─ Composer: campo de texto livre + enviar

```

**Fluxos principais da interface:**
* **Registrar** — "Comprei um jogo por 150" → Cents categoriza → cartão de lançamento na bolha → resumos atualizam.
* **Ambiguidade** — "gastei 200 no mercado ontem à noite" pode ser Mercado ou Lazer → Cents faz UMA pergunta curta e só grava depois da resposta.
* **Alerta** — ao cruzar 80% do limite da categoria, o Cents manda uma mensagem logo após o registro.
* **Meta/limite** — "quero economizar 800 esse mês", "meu limite de comida é 600".
* **Relatório** — "me manda o relatório do mês" → PDF gerado e baixado, com confirmação na conversa.

### 2. Comportamento do bot no "gasto por impulso"

**Regra central: zero culpa, foco no que sobra.**

| Situação | O que o Cents faz | O que nunca faz |
|---|---|---|
| Gasto grande fora do padrão | Registra primeiro, comenta depois, em uma frase | Perguntar "você precisava disso?" |
| Categoria acima de 80% | Mostra quanto ainda cabe e sugere um ajuste concreto para o resto do mês | Dizer que a pessoa "estourou" ou "exagerou" |
| Vários impulsos seguidos | Aponta o padrão de forma neutra e oferece um teto para os próximos dias | Comparar com outros meses de forma punitiva |
| Meta ameaçada | Recalcula quanto por dia ainda dá para guardar | Declarar a meta perdida |

**Exemplos de fala do Cents:**
* "Anotado: R$ 150 em Lazer 🎮 Ainda sobram R$ 90 na categoria esse mês — se segurar as próximas compras pro fim do mês, a meta continua de pé."
* "Chegou em 80% de Alimentação. Restam R$ 120 para 9 dias, ou seja ~R$ 13 por dia. Dá tranquilo se o mercado grande já estiver feito."
* "Terceira compra rápida essa semana. Que tal deixar as próximas para sexta? Aí a gente vê o total junto."
* O tom definido é casual, segunda pessoa, frases curtas, no máximo um emoji, sempre terminando com uma ação possível.

## 📸 Resultado Final
<img width="1594" height="783" alt="image" src="https://github.com/user-attachments/assets/af7d8a66-3e7a-4122-9cd6-60af14b95e53" />

## 💡 Reflexão sobre o Processo (Vibe Coding)

**O que funcionou bem:**
A capacidade da IA de pegar uma ideia abstrata e transformá-la em funcionalidades lógicas e um escopo fechado de MVP foi impressionante. Definir a "Persona" do agente no prompt logo de cara evitou que o app ficasse com aquela cara de "sistema de banco", mantendo a vibe amigável que eu queria. O teste prático no Lovable comprovou que a arquitetura idealizada é totalmente viável.

**O que não funcionou como o esperado:**
Em prompts iniciais menos detalhados, a IA tendia a adicionar funcionalidades complexas demais (como integração com Open Finance ou leitura de QR Code de notas fiscais), o que fugia completamente do conceito de MVP rápido.

**O que aprendi sobre conversar com IAs:**
Aprendi que a IA atua melhor como uma "funiladora de ideias" do que como uma criadora do zero. O conceito de *Vibe Coding* exige que você seja o Diretor de Arte e o Arquiteto de Produto. Se você não fornecer os "limites" (o que NÃO fazer), a IA vai expandir o escopo indefinidamente. A clareza da intenção e a imposição de restrições técnicas são os verdadeiros diferenciais para obter resultados profissionais.
