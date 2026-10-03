# FindHome Pets — Diagramas

Diagramas atualizados de acordo com os **fluxos simulados no protótipo**.
Os diagramas estão em [Mermaid](https://mermaid.js.org/) e são renderizados automaticamente pelo GitHub. Versões em imagem (PNG e SVG) de cada um estão em [`docs/img/`](img/), prontas para slides ou relatório.

**Participantes usados nos diagramas de sequência**

| Participante | No protótipo |
|---|---|
| Adotante / Protetor | Pessoa usando o app |
| App (telas) | Telas e formulários (`SCREENS`, `FORMS`) |
| API simulada | Regras de negócio que, na versão final, ficarão no servidor (objeto `API`) |
| Armazenamento local | `localStorage` do navegador, no lugar do banco de dados |

---

## 1. Diagramas de sequência

### 1.1 Login

```mermaid
sequenceDiagram
    autonumber
    actor U as Usuário
    participant T as App (telas)
    participant A as API simulada
    participant B as Armazenamento local

    U->>T: Informa e-mail e senha e toca em "Entrar"
    T->>T: Valida campos (obrigatórios e formato do e-mail)
    alt Campos inválidos
        T-->>U: Mostra erro em cada campo
    else Campos válidos
        T->>A: login(email, senha)
        A->>B: Busca usuário pelo e-mail
        B-->>A: Usuário (ou nenhum)
        alt Usuário não existe ou senha incorreta
            A-->>T: erro "E-mail ou senha incorretos"
            T-->>U: Exibe alerta de erro
        else Credenciais corretas
            A->>B: Grava sessão (usuário logado)
            A-->>T: Dados do usuário
            alt Perfil adotante
                T-->>U: Abre a tela Início (lista de pets)
            else Perfil protetor
                T-->>U: Abre o Painel do protetor
            end
        end
    end
```

### 1.2 Cadastro de conta

```mermaid
sequenceDiagram
    autonumber
    actor V as Visitante
    participant T as App (telas)
    participant A as API simulada
    participant B as Armazenamento local

    V->>T: Toca em "Criar conta"
    T-->>V: Exibe formulário de cadastro
    V->>T: Escolhe perfil (adotante ou protetor) e preenche os dados
    V->>T: Toca em "Criar conta"
    T->>T: Valida nome, e-mail, celular, cidade, senha, confirmação e termos
    alt Algum campo inválido
        T-->>V: Mostra mensagens nos campos com problema
    else Dados válidos
        T->>A: cadastrar(dados)
        A->>B: Verifica se o e-mail já existe
        alt E-mail já cadastrado
            A-->>T: erro no campo e-mail
            T-->>V: "Já existe uma conta com este e-mail"
        else E-mail livre
            A->>B: Salva novo usuário e abre sessão
            A-->>T: Usuário criado
            T-->>V: Boas-vindas e tela inicial do perfil escolhido
        end
    end
```

### 1.3 Solicitar adoção (fluxo principal do adotante)

```mermaid
sequenceDiagram
    autonumber
    actor AD as Adotante
    participant T as App (telas)
    participant A as API simulada
    participant B as Armazenamento local

    AD->>T: Busca e filtra pets na tela Início
    T->>A: petsDisponiveis() + filtros
    A->>B: Lê pets com status "disponível"
    B-->>A: Lista de pets
    A-->>T: Pets filtrados, ordenados por distância
    T-->>AD: Exibe cards dos pets
    AD->>T: Abre o pet e toca em "Quero adotar"
    T->>A: reqAtiva(adotante, pet)
    A-->>T: Nenhum pedido ativo
    T-->>AD: Exibe questionário de adoção
    AD->>T: Responde e toca em "Enviar pedido"
    T->>T: Valida respostas (inclui regra de telas para apartamento)
    alt Respostas incompletas
        T-->>AD: Destaca os campos pendentes
    else Respostas válidas
        T->>A: solicitar(adotante, pet, respostas)
        A->>A: Gera protocolo FH-AAAA-NNNN
        A->>B: Salva pedido com status "Em análise"
        A-->>T: Pedido criado
        T-->>AD: Tela "Pedido enviado!" com protocolo
        AD->>T: Toca em "Acompanhar pedido"
        T-->>AD: Linha do tempo do pedido
    end
```

### 1.4 Avaliar pedido (fluxo principal do protetor)

```mermaid
sequenceDiagram
    autonumber
    actor P as Protetor / ONG
    participant T as App (telas)
    participant A as API simulada
    participant B as Armazenamento local
    actor AD as Adotante

    P->>T: Abre "Pedidos" (pendentes)
    T->>A: reqsDoProtetor(protetor)
    A->>B: Lê pedidos dos pets deste protetor
    B-->>A: Pedidos
    A-->>T: Lista de pedidos
    T-->>P: Exibe pedidos pendentes
    P->>T: Abre um pedido e lê as respostas
    alt Aprovar
        P->>T: Toca em "Aprovar" e escreve mensagem (opcional)
        T-->>P: Pede confirmação
        P->>T: Confirma
        T->>A: responder(pedido, aprovar = sim, mensagem)
        A->>B: Pedido = "Aprovada" e pet = "Adotado"
        A->>B: Outros pedidos em análise do mesmo pet = "Recusada"
        A-->>T: Quantidade de pedidos recusados automaticamente
        T-->>P: Mostra contato do adotante e aviso de sucesso
    else Recusar
        P->>T: Toca em "Recusar" e informa o motivo
        P->>T: Confirma
        T->>A: responder(pedido, aprovar = não, motivo)
        A->>B: Pedido = "Recusada"
        T-->>P: Aviso "Pedido recusado"
    end
    Note over AD,T: Na próxima vez que abrir "Pedidos"
    AD->>T: Abre o pedido
    T->>A: req(id)
    A-->>T: Status atualizado e mensagem do protetor
    T-->>AD: Status e, se aprovado, contato do protetor
```

### 1.5 Cadastrar pet (protetor)

```mermaid
sequenceDiagram
    autonumber
    actor P as Protetor / ONG
    participant T as App (telas)
    participant A as API simulada
    participant B as Armazenamento local

    P->>T: Toca em "Cadastrar"
    T-->>P: Exibe formulário do pet
    P->>T: Preenche dados, saúde, personalidade e história
    P->>T: Toca em "Publicar pet"
    T->>T: Valida campos (personalidade de 1 a 3, história com 30+ caracteres)
    alt Dados inválidos
        T-->>P: Mostra erros nos campos
    else Dados válidos
        T->>A: cadastrarPet(protetor, dados)
        A->>B: Salva pet com status "disponível"
        A-->>T: Pet criado
        T-->>P: Abre a página do pet publicado
    end
```

---

## 2. Diagramas de atividade

### 2.1 Jornada do adotante: da entrada à adoção

```mermaid
flowchart TD
    I((Início)) --> O[Abrir o app]
    O --> S{Já tem sessão?}
    S -- Sim --> H[Tela Início]
    S -- Não --> OB[Onboarding] --> L{Tem conta?}
    L -- Não --> C[Preencher cadastro] --> VC{Dados válidos?}
    VC -- Não --> C
    VC -- Sim --> H
    L -- Sim --> LG[Informar e-mail e senha] --> VL{Credenciais corretas?}
    VL -- Não --> LG
    VL -- Sim --> H
    H --> F[Buscar e filtrar pets]
    F --> R{Encontrou um pet?}
    R -- Não --> F
    R -- Sim --> D[Ver detalhes do pet]
    D --> FV{Favoritar?}
    FV -- Sim --> FA[Adicionar aos favoritos] --> QA
    FV -- Não --> QA{Quer adotar?}
    QA -- Não --> F
    QA -- Sim --> PA{Já tem pedido ativo para este pet?}
    PA -- Sim --> VP[Ver pedido existente] --> AC
    PA -- Não --> Q[Responder questionário] --> VQ{Respostas válidas?}
    VQ -- Não --> Q
    VQ -- Sim --> EN[Enviar pedido e receber protocolo]
    EN --> AC[Acompanhar pedido]
    AC --> ST{Status do pedido}
    ST -- Em análise --> CA{Cancelar?}
    CA -- Sim --> CN[Pedido cancelado] --> F
    CA -- Não --> AC
    ST -- Recusada --> F
    ST -- Aprovada --> CT[Ver contato do protetor e combinar a entrega]
    CT --> FIM((Fim))
```

### 2.2 Avaliação de pedido pelo protetor

```mermaid
flowchart TD
    I((Início)) --> LG[Entrar como protetor]
    LG --> PN[Painel: ver pedidos pendentes]
    PN --> TP{Há pedidos pendentes?}
    TP -- Não --> CP[Cadastrar novo pet ou aguardar] --> FIM((Fim))
    TP -- Sim --> AB[Abrir pedido]
    AB --> LR[Ler respostas do adotante]
    LR --> DC{Decisão}
    DC -- Aprovar --> MA[Escrever mensagem opcional] --> CF1{Confirma?}
    CF1 -- Não --> LR
    CF1 -- Sim --> AP[Pedido aprovado]
    AP --> PT[Pet marcado como Adotado]
    PT --> OU{Há outros pedidos pendentes para o pet?}
    OU -- Sim --> RA[Recusar automaticamente com aviso] --> LC
    OU -- Não --> LC[Liberar contatos entre adotante e protetor]
    LC --> PN
    DC -- Recusar --> MR[Informar motivo] --> CF2{Confirma?}
    CF2 -- Não --> LR
    CF2 -- Sim --> RC[Pedido recusado] --> PN
```

### 2.3 Ciclo de vida do pedido de adoção

```mermaid
stateDiagram-v2
    [*] --> Enviado: adotante envia questionário
    Enviado --> EmAnalise: protetor recebe
    EmAnalise --> Aprovada: protetor aprova
    EmAnalise --> Recusada: protetor recusa
    EmAnalise --> Recusada: outro pedido do mesmo pet foi aprovado
    EmAnalise --> Cancelada: adotante cancela
    Aprovada --> [*]
    Recusada --> [*]
    Cancelada --> [*]

    state "Em análise" as EmAnalise
```
