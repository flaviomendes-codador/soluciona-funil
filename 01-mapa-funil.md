# Funil de Vendas IA — Clínica de Cirurgia Bucomaxilofacial
**Versão:** 1.0 | **Criado por:** Orion (@aios-master) | **Data:** 2026-03-02

---

## Visão Geral do Funil

```
┌─────────────────────────────────────────────────────────────────────┐
│                        TOPO DO FUNIL                                │
│                     Geração de Tráfego                              │
└─────────────────────────┬───────────────────────────────────────────┘
                          │
              ┌───────────▼───────────┐
              │   Meta Ads / Google   │
              │  Segmentação local    │
              │  Raio de 5-15km da    │
              │       clínica         │
              └───────────┬───────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────────────────┐
│                     LANDING PAGE                                    │
│                                                                     │
│  • Headline com dor/oferta específica (ex: "Sorriso Perfeito        │
│    em 1 Consulta — Agende Agora com Avaliação Grátis")              │
│  • Provas sociais (fotos, vídeos, depoimentos)                      │
│  • Oferta de entrada (avaliação gratuita / consulta diagnóstico)    │
│  • CTA principal → Formulário embutido                              │
└─────────────────────────┬───────────────────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────────────────┐
│                    FORMULÁRIO DE CAPTURA                            │
│                                                                     │
│  Campos coletados:                                                  │
│  → Nome completo                                                    │
│  → WhatsApp (obrigatório — canal principal)                         │
│  → Procedimento de interesse (dropdown — BMF):                       │
│     [ ] Cirurgia Ortognática (harmonização facial)                  │
│     [ ] Implante Zigomático / All-on-4 / All-on-6                  │
│     [ ] Enxerto Ósseo                                               │
│     [ ] Remoção de Cisto ou Tumor                                   │
│     [ ] Trauma Facial (fratura, reconstrução)                       │
│     [ ] Extração de Siso Incluso/Complicado                         │
│     [ ] Distúrbio da ATM (articulação)                              │
│     [ ] Indicação de dentista/médico (sem saber qual)               │
│     [ ] Outro / Não sei ainda                                       │
│  → Como chegou até nós (rastreamento):                              │
│     [ ] Indicação de dentista / médico                              │
│     [ ] Indicação de familiar / amigo                               │
│     [ ] Google / Pesquisa online                                    │
│     [ ] Instagram / Redes Sociais                    │
│  → Urgência (dropdown): Urgente / Esta semana / Este mês / Só       │
│    pesquisando                                                      │
│                                                                     │
│  → Webhook dispara imediatamente para o pipeline                    │
└─────────────────────────┬───────────────────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────────────────┐
│               FUNNELMETRICS — QUALIFICAÇÃO DE LEAD                  │
│                    (Sistema proprietário)                           │
│                                                                     │
│  Scoring automático do lead com base em:                            │
│  → Procedimento solicitado (ticket médio estimado — BMF)             │
│     • Implante Zigomático / All-on = 5pts (R$25k-80k)              │
│     • Cirurgia Ortognática = 5pts (R$15k-40k)                      │
│     • Enxerto Ósseo = 4pts (R$8k-20k)                              │
│     • Remoção Cisto/Tumor = 4pts (urgência + ticket)               │
│     • Trauma Facial = 5pts (urgência máxima)                       │
│     • Siso Incluso = 2pts (ticket menor, mas alto volume)           │
│     • Distúrbio ATM = 3pts                                         │
│     • Indicação sem saber = 3pts (qualificar na conversa)           │
│  → Origem da indicação                                              │
│     • Dentista/Médico = +2pts (já tem necessidade confirmada)       │
│     • Familiar/Amigo = +1pt                                         │
│     • Google/Redes = 0pts adicional             │
│  → Urgência declarada                                               │
│     • Urgente = 5pts | Esta semana = 3pts | Este mês = 2pt          │
│     • Só pesquisando = 0pts                                         │
│  → Origem do tráfego (taxa hist. de conversão por canal)            │
│                                                                     │
│  Classificação do lead:                                             │
│                                                                     │
│  🔴 QUENTE (7-10pts) → Call de 30min com o especialista            │
│     ↳ IA propõe horários, Dr. [Nome] fecha na call                 │
│                                                                     │
│  🟡 MORNO  (3-6pts)  → Secretária IA imediata (< 2 min)            │
│  🟢 FRIO   (0-2pts)  → Nutrição automática, sem atendimento agora  │
│                                                                     │
│  Output: Lead classificado → roteado para fila correta              │
└────────┬──────────────────────────┬──────────────┬─────────────────┘
         │                    │              │
       QUENTE              MORNO         FRIO (0-2pts)
         │                    │              │
         ▼                    ▼              ▼
┌─────────────────┐  ┌─────────────────┐  ┌─────────────────────┐
│  CALL C/ DOUTOR │  │  FILA SECRETÁRIA │  │  SEQUÊNCIA DE       │
│  (30 min)       │  │  IA (WhatsApp)   │  │  NUTRIÇÃO           │
│                 │  │  < 2 min        │  │  (7 FUs / 21 dias)  │
│  IA propõe     │  └────────┬─────────┘  └─────────────────────┘
│  horários do   │           │
│  Dr. disponível│           │
│  Lead escolhe  │           │
│  Calendário    │           │
│  confirmado    │           │
└────────┬────────┘           │
         │                        │
         ▼                        ▼
        │
        ▼
┌─────────────────────────────────────────────────────────────────────┐
│                   SECRETÁRIA IA — ATENDIMENTO                       │
│                   (N8N + WhatsApp + LLM)                            │
│                                                                     │
│  Passo 1 — ABERTURA (dentro de 2 min após formulário)               │
│  "Olá [Nome]! Sou a Ana, assistente da [Clínica]. Vi que você tem   │
│  interesse em [procedimento]. Posso te ajudar a agendar uma         │
│  avaliação gratuita para hoje ou amanhã?"                           │
│                                                                     │
│  Passo 2 — QUALIFICAÇÃO CONVERSACIONAL                              │
│  • Confirmar necessidade (aprofundar)                               │
│  • Identificar objeções iniciais (preço, tempo, medo)               │
│  • Coletar contexto adicional (tratamento anterior, urgência real)  │
│                                                                     │
│  Passo 3 — APRESENTAÇÃO DA SOLUÇÃO                                  │
│  • Apresentar o procedimento de forma acessível                     │
│  • Highlight da oferta de entrada (avaliação grátis)                │
│  • Ancoragem de valor (resultado x investimento)                    │
│                                                                     │
│  Passo 4 — TENTATIVA DE FECHAMENTO                                  │
│  "Qual o melhor dia para você? Temos horários amanhã às 10h,        │
│  14h ou 16h. O que funciona melhor?"                                │
└─────────────────────┬────────────────────┬──────────────────────────┘
                      │                    │
              AGENDOU ✅            NÃO AGENDOU ❌
                      │                    │
                      ▼                    ▼
┌─────────────────────────┐  ┌──────────────────────────────────────┐
│   FLUXO PÓS-VENDA       │  │   FLUXO DE FOLLOW-UP (7 tentativas) │
│   (Lead Convertido)     │  │   (Lead Não-Convertido)              │
└─────────────────────────┘  └──────────────────────────────────────┘
```

---

## Fluxo A — Pós-Venda (Fechou / Agendou)

```
AGENDAMENTO CONFIRMADO
        │
        ▼
┌───────────────────────────────────────────────────────────────────┐
│  D+0  → Confirmação imediata do agendamento                       │
│         "Perfeito [Nome]! Sua consulta está confirmada para        │
│          [data/hora]. Endereço: [Endereço]. Qualquer dúvida,       │
│          estou aqui!"                                             │
│                                                                   │
│  D-1  → Lembrete 24h antes                                        │
│         "Olá [Nome]! Lembrando que sua avaliação é amanhã às      │
│          [hora]. Alguma dúvida antes de vir?"                     │
│                                                                   │
│  D0   → Lembrete 2h antes                                         │
│         "Daqui a pouco te vemos! Só confirmando sua presença     │
│          para [hora]. 😊"                                         │
└───────────────────────────────────────────────────────────────────┘
        │
        ▼ (após consulta — D+1)
┌───────────────────────────────────────────────────────────────────┐
│  FOLLOW-UP PÓS-CONSULTA                                           │
│                                                                   │
│  D+1  → "Como foi sua experiência? Ficou com alguma dúvida        │
│          sobre o tratamento apresentado?"                         │
│                                                                   │
│  D+7  → Nutrição / Educação                                       │
│         Conteúdo sobre o procedimento de interesse               │
│         (benefícios, cuidados, FAQ)                               │
│                                                                   │
│  D+15 → Lembrete de retorno / Upsell                              │
│         "Está na hora do retorno! Aproveite para também           │
│          conhecer nosso [outro procedimento complementar]"        │
│                                                                   │
│  D+30 → Upsell / Cross-sell                                       │
│         Oferta de procedimento complementar com condição especial │
│         para paciente existente                                   │
│                                                                   │
│  D+90 → Indicação / Referral                                      │
│         "Você conhece alguém que também gostaria de um sorriso    │
│          renovado? Indica e ganhe [benefício]!"                   │
└───────────────────────────────────────────────────────────────────┘
```

---

## Fluxo B — Follow-Up Não-Convertido (7 Tentativas)

```
NÃO AGENDOU
     │
     ▼
┌───────────────────────────────────────────────────────────────────┐
│  SEQUÊNCIA DE 7 FOLLOW-UPS                                        │
│                                                                   │
│  FU1 → D+0 (2h após não-resposta ou recusa)                       │
│        Canal: WhatsApp                                            │
│        Abordagem: Reforço do benefício principal                  │
│                                                                   │
│  FU2 → D+1 (manhã)                                                │
│        Canal: WhatsApp                                            │
│        Abordagem: Prova social (antes/depois, depoimento)         │
│                                                                   │
│  FU3 → D+2 (tarde)                                                │
│        Canal: WhatsApp                                            │
│        Abordagem: Quebra de objeção de preço (parcelamento)       │
│                                                                   │
│  FU4 → D+4 (manhã)                                                │
│        Canal: WhatsApp                                            │
│        Abordagem: Urgência / Escassez (vagas limitadas)           │
│                                                                   │
│  FU5 → D+7 (tarde)                                                │
│        Canal: WhatsApp                                            │
│        Abordagem: Nova oferta / Bônus extra para decidir         │
│                                                                   │
│  FU6 → D+14 (manhã)                                               │
│        Canal: WhatsApp                                            │
│        Abordagem: "Última tentativa" — pergunta aberta sobre      │
│        o que impede (abre conversa para negociação)               │
│                                                                   │
│  FU7 → D+21                                                       │
│        Canal: WhatsApp                                            │
│        Abordagem: Conteúdo de valor (não venda) + manter          │
│        relacionamento para decisão futura                         │
│                                                                   │
│  APÓS FU7: Lead vai para lista de reengajamento trimestral        │
└───────────────────────────────────────────────────────────────────┘
```

---

## Métricas-Chave do Funil

| Etapa | Métrica | Meta |
|-------|---------|------|
| Tráfego → LP | CTR do anúncio | > 2% |
| LP → Formulário | Taxa de conversão LP | > 15% |
| Formulário → Qualificação | % Leads Quentes/Mornos | > 60% |
| Qualificação → Atendimento IA | Tempo de resposta | < 2 min |
| Atendimento → Agendamento | Taxa de conversão IA | > 30% |
| Follow-up → Agendamento | Taxa de resgate (FU1-7) | > 15% |
| Consulta → Tratamento | Taxa de fechamento presencial | > 50% |
| **CPL Qualificado** | **Custo por Lead que Agendou** | **Meta da clínica** |

---

## Integração FunnelMetrics no Funil

```
Formulário LP
      │
      ▼ (webhook POST)
FunnelMetrics API
      │
      ├── Calcula score do lead
      ├── Registra origem (utm_source, utm_medium, utm_campaign)
      ├── Atualiza dashboard em tempo real
      │   └── Custo por Lead Qualificado (CPL-Q)
      │   └── Funil de conversão por etapa
      │
      ▼ (resposta JSON com score + classificação)
N8N Workflow
      │
      ├── Score >= 4 → Fila secretária IA (prioritária)
      └── Score < 4  → Fila de nutrição (automática)
```

---

*Documento gerado por Orion (@aios-master) — Synkra AIOS*
