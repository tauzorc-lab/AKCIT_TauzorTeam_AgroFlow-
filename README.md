# AKCIT_TauzorTeam_AgroFlow-
MVP_01

# AgroFlow B2B — Documentação Completa do Projeto
> Plataforma B2B de comercialização agroalimentar para o corredor **Petrolina (PE) · Juazeiro (BA) · Recife (PE)**.
>
> Documento único, autocontido: produto, inteligência artificial e engenharia.
> Versão 1.0 · Base: novembro de 2026
![status](https://img.shields.io/badge/status-em%20desenvolvimento-8a6a2f)
![stack](https://img.shields.io/badge/React-19-1f2937)
![stack](https://img.shields.io/badge/TypeScript-5.8-1f2937)
![stack](https://img.shields.io/badge/Tailwind-3.4-1f2937)
![ia](https://img.shields.io/badge/IA-assistentes%20locais%20%2B%20roadmap-6b7f3a)
---
## Sumário
**Parte I — Produto**
1. [Visão geral](#1-visão-geral)
2. [O problema que resolvemos](#2-o-problema-que-resolvemos)
3. [Como funciona](#3-como-funciona)
4. [Telas e rotas](#4-telas-e-rotas)
5. [Funcionalidades por jornada](#5-funcionalidades-por-jornada)
**Parte II — Inteligência Artificial**
6. [IA hoje](#6-ia-hoje)
7. [Demandas de IA (IA-01 a IA-16)](#7-demandas-de-ia)
8. [Arquitetura de referência de IA](#8-arquitetura-de-referência-de-ia)
9. [Conjunto de avaliação](#9-conjunto-de-avaliação)
10. [Fases de entrega da IA](#10-fases-de-entrega-da-ia)
11. [Riscos transversais](#11-riscos-transversais)
**Parte III — Engenharia**
12. [Arquitetura técnica](#12-arquitetura-técnica)
13. [Modelo de dados](#13-modelo-de-dados)
14. [Como rodar o projeto](#14-como-rodar-o-projeto)
15. [Estrutura de pastas](#15-estrutura-de-pastas)
16. [Roadmap de produto](#16-roadmap-de-produto)
17. [Segurança, privacidade e LGPD](#17-segurança-privacidade-e-lgpd)
18. [Como contribuir](#18-como-contribuir)
19. [Licença](#19-licença)
---
# PARTE I — PRODUTO
## 1. Visão geral
O AgroFlow é um marketplace B2B de hortifruti com foco em **negociação qualificada**, e não apenas em vitrine de produtos. A plataforma parte de uma premissa simples: no agro, a negociação nasce no WhatsApp, em texto livre, desorganizada e sem rastro. O produto transforma essa conversa em **oferta estruturada**, **demanda estruturada**, **propostas comparáveis** e, por fim, em **pedido com conferência, documentação, doca e liquidação**.
Três pilares:
| Pilar | O que entrega |
| --- | --- |
| **Estruturado** | Toda mensagem comercial vira registro auditável com campos, unidades e prazos consistentes |
| **Comparável** | Preço unitário normalizado por quilo, frete separado e custo total entregue lado a lado |
| **Rastreável** | Trilha de auditoria append-only alimentada por agendamento, conferência e liquidação |
---
## 2. O problema que resolvemos
- **Informalidade da cotação.** Preço por caixa, por quilo, por tonelada e por pallet misturados na mesma conversa, sem base comum de comparação.
- **Custo entregue invisível.** O lote barato com frete caro quase nunca é o mais barato no destino.
- **Documento em falta só na portaria.** NF-e, GTV e laudo de resíduos aparecem quando o caminhão já está na doca.
- **Glosa sem memória.** O desconto por refugo é combinado verbalmente e não sobrevive ao mês seguinte.
- **Prazo de pagamento opaco.** D+14, D+21 e D+28 sem espelho de cálculo visível para o fornecedor.
---
## 3. Como funciona
```
Vendedor                                      Comprador
   │                                              │
   │  "15 t de manga Kent, caixa 20 kg,           │  "18 t de manga Palmer para
   │   R$ 68 a caixa, Petrolina, dia 20"          │   Recife até 28/11, extra"
   ▼                                              ▼
Assistente de oferta  ──►  Oferta estruturada   Assistente de compra ──► Demanda + RFQ
   │                                              │
   │  Calculadora de preço (custo → preço)        │  Convite a fornecedores
   ▼                                              ▼
Oferta publicada  ────────────────────────►  Propostas recebidas
                                                 │
                                                 ▼
                                     Comparador multicritério
                                     (preço · frete · prazo · qualidade · pagamento)
                                                 │
                                                 ▼
                                    Aprovação → Pedido → Conferência
                                                 │
                        ┌────────────────────────┼────────────────────────┐
                        ▼                        ▼                        ▼
                   Documentos                Doca / check-in         Liquidação
                   (NF-e, GTV, laudo)        (janela, placas)        (glosa → líquido)
                                                 │
                                                 ▼
                                          Trilha de auditoria
```
---
## 4. Telas e rotas
| Rota | Tela | Objetivo |
| --- | --- | --- |
| `/` | Home institucional | Proposta de valor, jornadas, vitrine de ofertas, comparador interativo, corredor logístico, governança |
| `/vendedor` | Jornada do vendedor | Assistente de oferta, cadastro validado, calculadora de preço, lista de ofertas |
| `/comprador` | Jornada do comprador | Demanda, RFQ, proposta comparada com pesos e critérios eliminatórios |
| `/pedidos` | Pedidos | Resumo, conferência de carga, documentos, doca/transporte, liquidação, auditoria |
| `/docas` | Gestão de docas | Painel por doca, janelas agendadas, dados de transporte, check-in de portaria |
| `/financeiro` | Financeiro & faturamento | Espelhos de liquidação, prazos D+14/21/28, extrato imprimível |
| `*` | 404 | Página não encontrada |
---
## 5. Funcionalidades por jornada
### 5.1 Vendedor
- **Assistente de oferta** — recebe a mensagem como ela é escrita no WhatsApp e devolve: produto, variedade, lote, quantidade (kg), embalagem, peso por embalagem, qualidade, localização, data de disponibilidade, preço (R$/kg) e condição de pagamento.
- **Conversão de unidade** — preço por caixa é convertido para R$/kg quando o peso da embalagem é conhecido; quando não é, o assistente **avisa** em vez de adivinhar.
- **Campos faltantes explícitos** — o que falta aparece como pendência, não como campo vazio silencioso.
- **Compradores compatíveis** — sugestão por produto e volume mensal, ordenada por volume.
- **Calculadora de preço determinística** — custo de produção + beneficiamento + embalagem por kg + frete, corrigido por perdas, aplicado margem e comparado ao preço mínimo; devolve preço por kg, preço por caixa, receita, lucro, margem efetiva e número de caixas.
- **Publicação de ofertas** com validação de campos obrigatórios.
### 5.2 Comprador
- **Registro de demanda** — produto, variedade, volume, qualidade, destino, prazo, orçamento por kg, condição de pagamento e exigência documental.
- **Assistente de compra** — mesma lógica de extração do lado da demanda, com sugestão de fornecedores para convite.
- **RFQ** — convite a fornecedores e recebimento de múltiplas propostas.
- **Comparador de propostas** — normalização min-max por critério, pesos ajustáveis, critérios eliminatórios (documentação, orçamento, prazo) e **explicação por critério**.
- **Recomendação explicada** — a plataforma recomenda, mas a decisão final é sempre do comprador.
### 5.3 Pedidos
- **Conferência de carga no CD** — Brix, calibre, temperatura do baú, firmeza e refugo, com glosa calculada sobre o refugo.
- **Checklist documental** — NF-e, GTV e Laudo de Resíduos com status *conforme*, *pendente* e *com ressalvas*, além de anexos simulados.
- **Doca e transporte** — janela, transportadora, motorista, placas, CNH, WhatsApp e check-in de portaria.
### 5.4 Docas
- Painel por doca e por janela de horário.
- Estados de check-in: aguardando pátio → em doca → descarregado.
- Visão alterada pela **organização ativa** (comprador ou fornecedor).
### 5.5 Financeiro
- **Espelho de liquidação**: acordado − frete retido − glosa = líquido.
- Prazos D+14, D+21 e D+28 com data prevista.
- Extrato imprimível e status *a faturar*, *em conciliação* e *liquidado*.
### 5.6 Transversal
- **Seletor de organização ativa** e papel (comprador/fornecedor).
- **Central de notificações** com filtro de lidas/não lidas e atalho para o registro de origem.
- **Trilha de auditoria append-only** — ator, organização, ação, entidade e detalhe.
---
# PARTE II — INTELIGÊNCIA ARTIFICIAL
## 6. IA hoje
> Estado atual: **assistentes locais baseados em regras determinísticas**, executados no navegador, sem chamada a APIs externas e sem enviar dados para terceiros.
| # | Capacidade | Onde vive | Tipo | Entrada | Saída |
| --- | --- | --- | --- | --- | --- |
| 1 | Assistente de oferta | `src/pages/seller/components/OfferAssistant.tsx` + `src/utils/offerParser.ts` | Regras + regex + normalização | Texto livre do vendedor | Oferta estruturada, pendências, avisos, compradores compatíveis |
| 2 | Assistente de compra | `src/pages/buyer/components/DemandAssistant.tsx` + `src/utils/demandParser.ts` | Regras + regex + normalização | Texto livre do comprador | Demanda estruturada, pendências, fornecedores sugeridos |
| 3 | Precificação assistida | `src/utils/pricing.ts` | Cálculo determinístico | Custos, embalagem, perdas, margem, mínimo | Preço por kg/caixa, lucro, margem efetiva, break-even de mínimo |
| 4 | Comparador multicritério | `src/utils/proposalScoring.ts` | Scoring normalizado min-max + regras eliminatórias | Demanda + propostas + pesos | Ranking, nota 0–100, motivo de eliminação, decomposição por critério |
| 5 | Comparador demonstrativo da home | `src/pages/home/components/Intelligence.tsx` | Scoring ao vivo no cliente | Pesos ajustáveis pelo visitante | Ranking e recomendação em tempo real |
### 6.1 Como a extração funciona (hoje)
1. **Normalização** — texto em minúsculas, remoção de acentos (NFD) para tolerar "exportação" / "exportacao".
2. *

3. 
