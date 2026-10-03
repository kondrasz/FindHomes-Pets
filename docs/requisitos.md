# FindHome Pets — Requisitos revisados

**Versão:** 2.0 (revisada após o protótipo funcional)
**Data:** 30/09/2026

Este documento revisa os requisitos do FindHome Pets com base no que foi **de fato implementado** no protótipo com dados mockados.
Cada requisito recebe um status:

| Status | Significado |
|---|---|
| ✅ Implementado | Funciona no protótipo como descrito |
| 🟡 Simulado | A tela e o fluxo existem, mas o comportamento depende de servidor e foi simulado (dados mockados / `localStorage`) |
| ⏳ Próxima etapa | Planejado, ainda não implementado |

---

## 1. Visão geral

O FindHome Pets é um aplicativo mobile que conecta **adotantes** a **protetores independentes e ONGs**, para que cães e gatos resgatados encontrem um lar responsável. O protetor publica os pets; o adotante encontra um pet, envia um pedido de adoção e acompanha a análise.

### 1.1 Atores

| Ator | Descrição |
|---|---|
| Visitante | Pessoa que ainda não entrou no app |
| Adotante | Usuário que procura um pet e envia pedidos de adoção |
| Protetor / ONG | Usuário que cadastra pets e avalia os pedidos recebidos |
| Sistema (API simulada) | Camada que aplica as regras de negócio e guarda os dados |

---

## 2. Requisitos funcionais

### 2.1 Acesso e conta

| ID | Requisito | Ator | Status | Observação |
|---|---|---|---|---|
| RF01 | O sistema deve apresentar uma introdução (onboarding) com a proposta do app na primeira abertura. | Visitante | ✅ Implementado | 3 passos, com opção "Pular" |
| RF02 | O usuário deve poder criar conta escolhendo o perfil **adotante** ou **protetor/ONG**, informando nome, e-mail, celular, cidade e senha. | Visitante | 🟡 Simulado | Conta salva no navegador |
| RF03 | O sistema deve validar os dados do cadastro (campos obrigatórios, e-mail válido, celular com DDD, senha com 6+ caracteres, confirmação de senha, aceite dos termos). | Sistema | ✅ Implementado | Mensagens por campo |
| RF04 | O sistema deve impedir cadastro com e-mail já existente. | Sistema | ✅ Implementado | |
| RF05 | O usuário deve poder entrar com e-mail e senha. | Visitante | 🟡 Simulado | Validação contra usuários mockados |
| RF06 | O sistema deve informar quando e-mail ou senha estiverem incorretos. | Sistema | ✅ Implementado | |
| RF07 | O usuário deve poder sair da conta. | Adotante, Protetor | ✅ Implementado | |
| RF08 | O usuário deve poder recuperar a senha por e-mail. | Visitante | ⏳ Próxima etapa | Botão exibe aviso |
| RF09 | O usuário deve poder visualizar seus dados de perfil. | Adotante, Protetor | ✅ Implementado | Edição fica para a próxima etapa |

### 2.2 Busca e descoberta (adotante)

| ID | Requisito | Ator | Status | Observação |
|---|---|---|---|---|
| RF10 | O adotante deve ver a lista de pets disponíveis, ordenada pela distância. | Adotante | 🟡 Simulado | Distâncias fixas nos dados mockados |
| RF11 | O adotante deve poder buscar pets por nome, bairro ou característica. | Adotante | ✅ Implementado | Busca em tempo real |
| RF12 | O adotante deve poder filtrar por espécie, porte, faixa de idade, sexo e distância. | Adotante | ✅ Implementado | Contador de filtros ativos |
| RF13 | O adotante deve ver os detalhes do pet: idade, sexo, porte, peso, saúde (vacinação, castração, vermifugação), personalidade, história e responsável. | Adotante | ✅ Implementado | Fotos substituídas por ilustrações |
| RF14 | O adotante deve poder favoritar e desfavoritar pets e ver a lista de favoritos. | Adotante | ✅ Implementado | |

### 2.3 Pedido de adoção (adotante)

| ID | Requisito | Ator | Status | Observação |
|---|---|---|---|---|
| RF15 | O adotante deve poder enviar um pedido de adoção respondendo a um questionário (moradia, imóvel, telas, moradores, outros animais, tempo sozinho, motivação e termo de ciência). | Adotante | ✅ Implementado | |
| RF16 | O sistema deve validar o questionário antes do envio. | Sistema | ✅ Implementado | |
| RF17 | O sistema deve gerar um número de protocolo para cada pedido. | Sistema | ✅ Implementado | Formato `FH-AAAA-NNNN` |
| RF18 | O adotante deve acompanhar seus pedidos com status e linha do tempo. | Adotante | ✅ Implementado | |
| RF19 | O adotante deve poder cancelar um pedido que ainda está em análise. | Adotante | ✅ Implementado | Com confirmação |
| RF20 | Quando o pedido for aprovado, o sistema deve liberar o contato do protetor para o adotante. | Sistema | ✅ Implementado | |
| RF21 | O usuário deve receber notificações push quando o status do pedido mudar. | Sistema | ⏳ Próxima etapa | Hoje o status é visto na tela de pedidos |

### 2.4 Gestão (protetor / ONG)

| ID | Requisito | Ator | Status | Observação |
|---|---|---|---|---|
| RF22 | O protetor deve ver um painel com pedidos pendentes, pets disponíveis e adoções concluídas. | Protetor | ✅ Implementado | |
| RF23 | O protetor deve poder cadastrar um pet com nome, espécie, sexo, idade, porte, peso, bairro, saúde, personalidade e história. | Protetor | 🟡 Simulado | Pet aparece imediatamente para os adotantes |
| RF24 | O protetor deve poder enviar fotos do pet. | Protetor | ⏳ Próxima etapa | Protótipo usa ilustração por cor de pelagem |
| RF25 | O protetor deve ver os pedidos recebidos, separados em pendentes e respondidos. | Protetor | ✅ Implementado | |
| RF26 | O protetor deve poder ver as respostas do adotante e **aprovar** ou **recusar** o pedido, com mensagem opcional. | Protetor | ✅ Implementado | Com confirmação |
| RF27 | O protetor deve poder editar ou remover um pet cadastrado. | Protetor | ⏳ Próxima etapa | |
| RF28 | Adotante e protetor devem poder conversar por chat dentro do app. | Ambos | ⏳ Próxima etapa | Substituído pela liberação de contato após aprovação |

---

## 3. Regras de negócio

| ID | Regra | Status |
|---|---|---|
| RN01 | Apenas usuários com perfil **adotante** podem enviar pedidos de adoção. | ✅ |
| RN02 | Um adotante pode ter **apenas um pedido ativo** (em análise ou aprovado) por pet. | ✅ |
| RN03 | Somente o protetor responsável pelo pet pode aprovar ou recusar os pedidos daquele pet. | ✅ |
| RN04 | Ao aprovar um pedido, o pet passa para **Adotado** e sai da lista de adoção. | ✅ |
| RN05 | Ao aprovar um pedido, os **demais pedidos em análise para o mesmo pet são recusados automaticamente**, com mensagem ao adotante. | ✅ |
| RN06 | O contato (telefone e e-mail) do protetor só é exibido ao adotante depois da aprovação, e vice-versa. | ✅ |
| RN07 | Para moradia em apartamento, o adotante deve informar a situação das telas de proteção. | ✅ |
| RN08 | Ciclo de vida do pedido: Enviado → Em análise → Aprovada, Recusada ou Cancelada. Só pedidos em análise podem ser cancelados, aprovados ou recusados. | ✅ |
| RN09 | O cadastro de pet exige ao menos uma característica de personalidade (máximo de 3) e uma história com 30+ caracteres. | ✅ |

---

## 4. Requisitos não funcionais

| ID | Requisito | Status | Como foi atendido |
|---|---|---|---|
| RNF01 | **Usabilidade:** interface mobile-first, com navegação por abas e no máximo 3 toques até o pedido de adoção a partir da tela inicial. | ✅ | Início → Pet → Quero adotar |
| RNF02 | **Identidade visual:** seguir o manual da marca (Nunito Sans; amarelo `#F8D978`, branco, azul `#4D8FCB` e `#285A84`). | ✅ | Tokens de cor e tipografia no CSS |
| RNF03 | **Acessibilidade:** textos com contraste adequado, foco visível no teclado, rótulos em campos e botões de ícone, respeito a "reduzir movimento". | ✅ | `aria-label`, `:focus-visible`, `prefers-reduced-motion` |
| RNF04 | **Responsividade:** funcionar em telas a partir de 360 px de largura. | ✅ | Testado em 390 px e desktop |
| RNF05 | **Compatibilidade:** rodar nos navegadores Chrome, Edge, Firefox e Safari atuais, sem instalação. | ✅ | HTML/CSS/JS puros |
| RNF06 | **Disponibilidade para teste:** acessível por link público gratuito. | ✅ | Link publicado / GitHub Pages |
| RNF07 | **Feedback:** toda ação importante exibe confirmação ou mensagem (toasts, telas de sucesso, erros por campo). | ✅ | |
| RNF08 | **Segurança:** senhas armazenadas com hash e comunicação via HTTPS. | ⏳ | No protótipo as senhas mockadas ficam em texto no navegador. Exigência para a versão com servidor |
| RNF09 | **Privacidade (LGPD):** dados pessoais só compartilhados entre adotante e protetor após aprovação; termo de uso aceito no cadastro. | 🟡 | Regra RN06 e aceite dos termos implementados; política de privacidade a redigir |
| RNF10 | **Persistência:** dados devem ser mantidos entre sessões. | 🟡 | `localStorage` do navegador; banco de dados na próxima etapa |

---

## 5. O que mudou em relação à versão anterior

- **Adicionado:** questionário de adoção estruturado (RF15), protocolo (RF17), cancelamento pelo adotante (RF19) e recusa automática dos outros pedidos (RN05). Surgiram ao simular o fluxo completo de ponta a ponta.
- **Adicionado:** painel do protetor com indicadores (RF22), para dar ao protetor uma visão rápida do que precisa de resposta.
- **Adiado:** chat (RF28), notificações push (RF21), fotos reais (RF24), edição/remoção de pets (RF27) e recuperação de senha (RF08). Dependem de servidor, armazenamento de arquivos ou serviços externos.
- **Ajustado:** contato entre adotante e protetor passa a ser liberado só após aprovação (RN06), como alternativa mais simples e segura ao chat nesta etapa.

> Conferir com os requisitos originais do Miro e acrescentar aqui qualquer item que não tenha sido coberto.
