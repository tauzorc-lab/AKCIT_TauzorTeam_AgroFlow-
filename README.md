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

2. **Dicionário de produtos** — catálogo de produtos e variedades do corredor.
3. **Expressões por campo** — volume (t/kg), embalagem (caixa/saco/engradado + peso), preço (R$ por kg ou por caixa), qualidade (extra/exportação/industrial/categoria), praça, data (dd/mm ou dd/mm/aaaa), pagamento (à vista, N dias, PIX, boleto).
4. **Validação de coerência** — preço por caixa sem peso da caixa gera aviso; caixas sem peso não viram kg.
5. **Contrato de saída** — `draft` (dados extraídos), `found` (rótulo + valor legível), `missing` (obrigatórios ausentes), `warnings` (ambiguidades).
### 6.2 Como a recomendação funciona (hoje)
- Cada proposta é pontuada por **preço entregue (preço + frete)**, **frete**, **qualidade**, **prazo de entrega** e **condição de pagamento**.
- Cada critério é normalizado no intervalo da própria carteira de propostas, com inversão quando menor é melhor.
- Pesos são definidos pelo comprador e normalizados pela soma.
- **Critérios eliminatórios** rodam antes da nota: documentação obrigatória ausente, custo entregue acima do orçamento, prazo maior que a data limite.
- A saída inclui **motivo textual de eliminação** e **quebra por critério** para que a decisão seja auditável.
### 6.3 Limites conhecidos
- Vocabulário fechado: produto fora do catálogo não é reconhecido.
- Sem compreensão de contexto: "a mesma coisa do mês passado" não é resolvido.
- Sem negociação: o assistente estrutura, não contra-argumenta.
- Sem aprendizado: nenhuma extração é usada para melhorar o modelo.
- Sem visão computacional e sem OCR: nada é lido de imagem ou PDF.
---
## 7. Demandas de IA
### 7.0 Princípios não negociáveis
1. **Humano no comando.** A IA estrutura, sugere e explica. Nunca aprova pedido, nunca altera preço, nunca libera pagamento.
2. **Explicável por padrão.** Toda recomendação mostra *por que* — critério, valor e peso.
3. **Degradação elegante.** Sem LLM disponível, o assistente por regras continua funcionando.
4. **Nada de alucinação numérica.** Preço, volume e prazo só são aceitos quando vieram do texto ou de um cálculo determinístico verificável.
5. **Dado sensível não sai sem contrato.** Nenhum dado de negociação vai para provedor externo sem aval e sem registro.
6. **Segredo só no servidor.** Chaves de IA vivem em segredo de backend, nunca no bundle do navegador.
**Prioridades:** `P0` bloqueia a próxima fase · `P1` alto valor · `P2` ganho incremental.
### 7.1 Resumo priorizado
| ID | Demanda | Prioridade | Horizonte | Dependência |
| --- | --- | --- | --- | --- |
| IA-01 | Extração robusta com LLM (function calling) + fallback por regras | P0 | Fase 1 | Backend/gateway de IA |
| IA-02 | Diálogo de esclarecimento (perguntar antes de assumir) | P0 | Fase 1 | IA-01 |
| IA-03 | Matching oferta ↔ demanda com ranking explicado | P0 | Fase 1 | Dados de oferta/demanda persistidos |
| IA-04 | Normalização inteligente de unidades e embalagens | P1 | Fase 2 | IA-01 |
| IA-05 | Assistente de negociação com contraproposta sugerida | P1 | Fase 2 | IA-03, aprovação humana |
| IA-06 | Previsão de preço por produto, praça e semana | P1 | Fase 2 | Histórico de negociações |
| IA-07 | Consolidação de cargas e sugestão de rota | P1 | Fase 3 | Docas + fretes |
| IA-08 | OCR de NF-e, GTV e laudo com validação cruzada | P1 | Fase 3 | Checklist documental |
| IA-09 | Visão computacional na conferência de carga | P2 | Fase 3 | Fotos + padrão de qualidade |
| IA-10 | Detecção de anomalia em glosa e liquidação | P2 | Fase 3 | Histórico de liquidação |
| IA-11 | Redação assistida (RFQ, proposta, mensagem ao motorista) | P2 | Fase 2 | IA-01 |
| IA-12 | Busca semântica em ofertas e demandas | P2 | Fase 2 | Catálogo indexado |
| IA-13 | Triagem de mensagens recebidas (intake WhatsApp/e-mail) | P2 | Fase 4 | Canal de entrada |
| IA-14 | Score de risco de crédito do comprador | P2 | Fase 4 | Histórico financeiro |
| IA-15 | Copiloto de docas (previsão de atraso e sugestão de janela) | P2 | Fase 4 | Agendamentos |
| IA-16 | Governança de IA: explicabilidade, avaliação e guarda-corpos | P0 | Contínuo | Todas |
### 7.2 Métricas de sucesso das demandas
| Métrica | Meta inicial |
| --- | --- |
| Precisão de extração por campo (produto, volume, preço) | ≥ 92% |
| Taxa de demanda/oferta que chega completa ao formulário | ≥ 80% |
| Redução de tempo para montar uma oferta | de ~9 min para ≤ 3 min |
| Recomendação aceita sem edição de pesos | ≥ 60% |
| Alucinação numérica (valor inexistente no texto) | 0 caso em auditoria |
| Custo de IA por negociação concluída | dentro do teto definido por fase |
---
### IA-01 · Extração robusta com LLM
**Prioridade:** P0 · **Fase:** 1 · **Depende de:** gateway de IA em Edge Function
**Contexto.** Hoje os assistentes usam dicionário fechado e expressões regulares. Funcionam bem para o vocabulário do corredor (manga, uva, cebola, melão, acerola), mas quebram com mensagem escrita às pressas: "me vê 400 cx daquelas de 20, entrega na quinta", "tenho 2 carreta de Palmer", "uns 18 mil kg".
**Objetivo.** Elevar a taxa de extração correta por campo para ≥ 92% em mensagens reais do corredor, mantendo o parser atual como caminho de fallback.
**Escopo.**
- Entra: produto, variedade, volume, unidade, embalagem, peso por embalagem, preço, qualidade, praça de origem/destino, data, condição de pagamento, exigência documental.
- Entra: resposta obrigatória em JSON validado contra schema.
- Entra: fallback automático ao parser por regras quando o modelo falha, estoura tempo ou devolve JSON inválido.
- Entra: confiança por campo.
- Não entra: escrita direta no banco sem confirmação do usuário; cálculo de preço final.
**Regras de negócio.**
1. Campo sem evidência no texto retorna `null` — proibido preencher por conveniência.
2. Preço só é aceito se o texto tiver valor monetário; conversão por caixa exige peso explícito.
3. Data sem ano assume o próximo ano em que a data ainda é futura.
4. Toda extração precisa ser aplicável ao formulário com um clique e reversível.
**Critérios de aceite**
- [ ] Resposta com o mesmo contrato do parser atual (`draft`, `found`, `missing`, `warnings`).
- [ ] Schema de resposta validado; resposta inválida nunca chega à interface.
- [ ] Latência p95 abaixo de 2,5 s para mensagens de até 400 caracteres.
- [ ] Fallback local acionado em timeout, erro 5xx e JSON inválido.
- [ ] Chave do provedor somente em segredo de backend.
- [ ] Teste de regressão com 300 mensagens anotadas, precisão ≥ 92% por campo principal.
- [ ] Custo por extração registrado e visível no painel interno.
**Métricas.** Precisão por campo · taxa de fallback · latência p95 · custo por extração · taxa de "aplicar ao formulário".
**Riscos.**
| Risco | Mitigação |
| --- | --- |
| Alucinação de preço ou volume | Campo sem evidência retorna nulo; validação cruzada com o texto |
| Custo descontrolado | Cache por hash de mensagem; limite por organização; modelo menor primeiro |
| Indisponibilidade do provedor | Fallback local sempre disponível |
| Vazamento de dado comercial | Contrato sem retenção de treino; anonimização de nomes |
---
### IA-02 · Diálogo de esclarecimento
**Prioridade:** P0 · **Fase:** 1 · **Depende de:** IA-01
**Contexto.** Quando falta um campo, o assistente atual lista pendências de forma passiva. O vendedor no campo lê a lista e não responde.
**Objetivo.** Transformar pendência em **uma pergunta por vez**, com opções clicáveis, até a oferta ou demanda ficar completa.
**Escopo.**
- Entra: ordem de prioridade das perguntas (produto → volume → embalagem → qualidade → praça → data → preço → pagamento).
- Entra: no máximo 3 perguntas por turno, uma por vez.
- Entra: opções sugeridas a partir do catálogo e do contexto.
- Entra: registro do que foi respondido versus inferido.
- Não entra: insistir mais de uma vez na mesma pergunta.
**Regras de negócio.**
1. Uma pergunta por mensagem, sempre com exemplo de resposta.
2. Campo já respondido não é perguntado de novo.
3. Ambiguidade de unidade ("400 cx" sem peso) é bloqueante para conversão, não para cadastro.
4. Após 3 rodadas sem progresso, o assistente entrega o formulário parcial e destaca o que falta.
**Critérios de aceite**
- [ ] Assistente mantém estado do rascunho entre turnos.
- [ ] Cada resposta atualiza visualmente o rascunho estruturado.
- [ ] Trilha de origem por campo: `texto`, `resposta` ou `inferido`.
- [ ] Comportamento idêntico ao aplicar ao formulário.
- [ ] Teste de usabilidade: oferta completa em ≤ 3 rodadas em 70% das sessões.
**Métricas.** Rodadas médias até completar · taxa de conclusão · abandono por etapa.
**Riscos.** Irritação com excesso de perguntas → limite rígido por turno e opções clicáveis.
---
### IA-03 · Matching oferta ↔ demanda
**Prioridade:** P0 · **Fase:** 1 · **Depende de:** ofertas e demandas persistidas no Backend
**Contexto.** O match atual cruza apenas produto e ordena por volume mensal. Não considera variedade, janela de entrega, capacidade, praça, histórico ou documentação.
**Objetivo.** Produzir um ranking de compatibilidade com justificativa por par oferta-demanda.
**Escopo.**
- Entra: compatibilidade por produto e variedade (exata, equivalente, incompatível).
- Entra: janela de disponibilidade × prazo da demanda.
- Entra: volume ofertado × volume demandado (cobertura parcial permitida).
- Entra: distância e frete estimado entre origem e destino.
- Entra: exigência documental.
- Entra: histórico de negociação (concluída, recusada, glosa média).
- Não entra: exclusão automática de fornecedor; apenas ordenação e sinalização.
**Regras de negócio.**
1. Incompatibilidade de produto é eliminação dura.
2. Cobertura parcial abaixo de 60% do volume é marcada como "carga parcial".
3. Recomendação sempre acompanhada de pelo menos 3 fatores de explicação.
4. Empate resolvido por menor custo entregue e depois

5. ## 8. Arquitetura de referência de IA
```
Navegador (React)
   │  supabase.functions.invoke('ai-extract', { body })
   ▼
Edge Function (gateway de IA)
   ├─ valida sessão do usuário e organização ativa
   ├─ aplica limite de uso e cache por hash da mensagem
   ├─ seleciona versão do prompt
   ├─ chama o provedor de LLM (chave só em segredo de backend)
   ├─ valida a resposta contra JSON Schema
   ├─ em falha, aciona o parser local como fallback
   ├─ grava em ai_runs (entrada, saída, latência, custo)
   └─ devolve o mesmo contrato do parser atual
   ▼
Interface aplica ao formulário apenas com clique do usuário
```
**Regras de arquitetura**
- Chave de provedor nunca no bundle do cliente.
- Resposta de modelo nunca é usada sem validação de schema.
- Fallback local sempre implementado — a IA generativa é melhoria, não dependência.
- Custo e latência medidos por chamada, não por sessão.
- Cada capacidade em função separada, para permitir evolução e reversão independentes.

- ## 9. Conjunto de avaliação
Para qualquer nova capacidade generativa, antes de publicar:
1. **Massa mínima:** 200 casos reais anonimizados por capacidade.
2. **Anotação dupla:** dois anotadores por caso; divergência resolvida por terceiro.
3. **Recortes obrigatórios:** mensagem curta (< 15 palavras), com erro de digitação, com gíria regional, com múltiplos produtos, com unidade ambígua.
4. **Métrica por campo:** precisão e recall separados — não usar apenas acurácia global.
5. **Portão de saída:** precisão ≥ 92% nos campos principais e **zero** alucinação numérica.
6. **Regressão:** todo caso que falhou vira caso permanente do conjunto.
