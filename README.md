# 🐾 FindHome Pets

> Conectando vidas, lares e solidariedade — da adoção responsável de cães, gatos e animais exóticos ao resgate de pets perdidos e apoio comunitário.

---

## 📌 1. Visão Geral

O **FindHome Pets** é um ecossistema projetado para unificar e desburocratizar a causa animal. O projeto valida de forma enxuta e integrada quatro frentes fundamentais:
1. Conexão assertiva entre adotantes e animais (incluindo espécies exóticas).
2. Mobilização rápida para localização de pets desaparecidos.
3. Educação e orientação prática sobre manejo e posse responsável.
4. Rede comunitária para doação de insumos e apoio financeiro direto a abrigos.

---

## 🎯 2. Escopo do MVP (Por Pilar)
```text
                              ┌───────────────────────────────┐
                              │        FINDHOME PETS          │
                              └───────────────┬───────────────┘
         ┌────────────────────────┬───────────┴───────────┬────────────────────────┐
         ▼                        ▼                       ▼                        ▼
   [ FINDMATCH ]            [ BACKHOME ]            [ CAREGUIDE ]            [ HELPHOME ]
     (Adoção)                (Perdidos)              (Cuidados)                (Doações)
```
### 🐾 Pilar 1: FindMatch (Adoção)
*   **Cadastro do Pet**: Suporte para cães, gatos e espécies exóticas (aves, répteis e roedores), contendo fotos, idade, porte, temperamento e campo para dados legais (ex.: anilha, microchip ou autorização de órgãos ambientais).
*   **Filtros Básicos**: Filtragem dinâmica por espécie, localização/cidade e porte.
*   **Deck de Cards (Swipe)**: Mecânica interativa de deslizar para a direita ("curtir") ou esquerda ("passar").
*   **Match & Contato**: Liberação de informações de contato direto ou link para WhatsApp da ONG/protetor responsável após confirmação de interesse.

### 🚨 Pilar 2: BackHome (Pets Perdidos)
*   **Cadastro de Alerta SOS**: Registro rápido de pet desaparecido com foto, nome, características visuais, cidade e ponto de fuga.
*   **Mural de Desaparecidos**: Feed visual de alertas ordenados por proximidade geográfica e cidade.
*   **Cartaz Automático**: Geração instantânea de banner digital formatado para compartilhamento em redes sociais (WhatsApp e Instagram Stories).

### 📚 Pilar 3: CareGuide (Central de Cuidados)
*   **Guias Rápidos por Espécie**: Cards informativos com diretrizes essenciais de manejo para convencionais e exóticos (alimentação permitida/tóxica, parâmetros de recinto/espaço e calendário de saúde).
*   **Checklist do Tutor**: Lista de checagem interativa com os itens indispensáveis antes de receber o animal.

### 🤝 Pilar 4: HelpHome (Doações)
*   **Mural de Insumos Físicos**: Espaço colaborativo para oferta e solicitação de itens (ex.: sobras de ração, caixas de transporte, medicamentos na validade, viveiros e terrários).
*   **Pix Direto para ONGs**: Exibição validada de chaves Pix e dados institucionais para transferência financeira direta aos abrigos e protetores parceiros.

---

## 🚫 3. Fora do Escopo (Fases Futuras)

*   Gateway interno ou processamento direto de pagamentos (o MVP utiliza transações diretas via Pix externo).
*   Logística e cálculo de frete para envio de insumos físicos.
*   Reconhecimento de imagem via IA para identificação automática de pets sumidos.
*   Sistema de telemedicina ou agendamento veterinário integrado.

---

## 📁 4. Estrutura do Repositório

```text
findhome-pets/
├── docs/
│   ├── brand/          # Logo, paleta de cores e tipografia
│   ├── uml/            # Diagramas de Casos de Uso e Classes
│   └── pitch.md        # Roteiro de apresentação
├── src/
│   ├── backend/        # Código-fonte da API
│   └── frontend/       # Interface da aplicação (Mobile/Web)
├── .gitignore
├── LICENSE
└── README.md 
