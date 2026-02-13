# 🚀 Sistema de Vagas Handcom

**Objetivo:** Sistema completo de gestão de vagas com integração Tally, triagem IA e site público.

---

## 📋 Escopo do Projeto

### Fase 1: Infraestrutura (MVP)
- [x] Configurar API Tally
- [ ] Criar skill `vagas` com comandos básicos
- [ ] Banco de dados local (vagas, candidatos)
- [ ] Template padrão de formulário Tally
- [ ] Webhook para receber candidatos

### Fase 2: Gestão de Vagas
- [ ] CRUD de vagas (criar, editar, pausar, encerrar)
- [ ] Auto-criação de form Tally ao criar vaga
- [ ] Campos configuráveis por vaga
- [ ] Status: rascunho, publicada, pausada, encerrada

### Fase 3: Triagem com IA
- [ ] Parsing de currículo (PDF/texto)
- [ ] Análise de fit com requisitos da vaga
- [ ] Score 0-100 por candidato
- [ ] Feedback automático por email
- [ ] Classificação: apto, talvez, não apto

### Fase 4: Dashboard
- [ ] Painel web local (ou consultas via chat)
- [ ] Filtros por vaga, score, status
- [ ] Timeline do candidato
- [ ] Notas e avaliações manuais

### Fase 5: Site Público de Vagas
- [ ] Site estático com listagem de vagas
- [ ] Deploy automático (GitHub Pages / Vercel)
- [ ] Cada vaga linka pro form Tally
- [ ] SEO básico

---

## 🏗️ Arquitetura

```
┌─────────────────┐      ┌──────────────┐      ┌───────────────┐
│   Henry/CLI     │──────│  Skill Vagas │──────│  SQLite DB    │
│   (comandos)    │      │  (Python)    │      │  vagas.db     │
└─────────────────┘      └──────┬───────┘      └───────────────┘
                                │
                    ┌───────────┼───────────┐
                    ▼           ▼           ▼
            ┌──────────┐  ┌──────────┐  ┌──────────┐
            │ Tally API│  │ Webhooks │  │ Site Gen │
            │(forms)   │  │(candidat)│  │ (Hugo)   │
            └──────────┘  └────┬─────┘  └────┬─────┘
                               │              │
                    ┌──────────▼──────────────▼───────┐
                    │     vagas.handcom.com.br        │
                    │     (GitHub Pages / Vercel)     │
                    └─────────────────────────────────┘
```

---

## 🔧 Componentes

### 1. Skill `vagas`
**Localização:** `~/.openclaw/workspace/skills/vagas/`

**Comandos:**
```bash
vagas listar                     # Lista vagas ativas
vagas criar <titulo>             # Cria nova vaga
vagas editar <id>                # Edita vaga existente
vagas publicar <id>              # Publica vaga (cria form Tally)
vagas pausar <id>                # Pausa vaga
vagas encerrar <id>              # Encerra vaga
vagas candidatos [vaga_id]       # Lista candidatos
vagas triagem <candidato_id>     # Executa triagem IA
vagas avaliar <cand_id> <nota>   # Avaliação manual
vagas site gerar                 # Gera site estático
vagas site publicar              # Publica site
```

### 2. Banco de Dados
**Tabelas:**

**vagas**
| Campo | Tipo | Descrição |
|-------|------|-----------|
| id | INTEGER PK | ID único |
| titulo | TEXT | Título da vaga |
| descricao | TEXT | Descrição completa |
| requisitos | TEXT | Lista de requisitos |
| beneficios | TEXT | Benefícios oferecidos |
| salario_min | REAL | Salário mínimo (opcional) |
| salario_max | REAL | Salário máximo (opcional) |
| tipo | TEXT | CLT, PJ, Estágio |
| modalidade | TEXT | Remoto, Híbrido, Presencial |
| local | TEXT | Cidade/Estado |
| departamento | TEXT | Área da vaga |
| status | TEXT | rascunho, publicada, pausada, encerrada |
| tally_form_id | TEXT | ID do form no Tally |
| tally_form_url | TEXT | URL pública do form |
| criado_em | DATETIME | Data de criação |
| publicado_em | DATETIME | Data de publicação |
| encerrado_em | DATETIME | Data de encerramento |

**candidatos**
| Campo | Tipo | Descrição |
|-------|------|-----------|
| id | INTEGER PK | ID único |
| vaga_id | INTEGER FK | ID da vaga |
| nome | TEXT | Nome completo |
| email | TEXT | Email |
| telefone | TEXT | Telefone |
| linkedin | TEXT | URL LinkedIn |
| curriculo_texto | TEXT | Texto do currículo |
| curriculo_url | TEXT | URL do arquivo |
| pretensao | REAL | Pretensão salarial |
| disponibilidade | TEXT | Disponibilidade |
| tally_submission_id | TEXT | ID da submissão Tally |
| score_fit | INTEGER | Score IA (0-100) |
| analise_ia | TEXT | Análise detalhada IA |
| status | TEXT | novo, em_analise, apto, talvez, nao_apto, contratado |
| notas | TEXT | Notas internas |
| criado_em | DATETIME | Data de aplicação |
| atualizado_em | DATETIME | Última atualização |

**avaliacoes**
| Campo | Tipo | Descrição |
|-------|------|-----------|
| id | INTEGER PK | ID único |
| candidato_id | INTEGER FK | ID do candidato |
| avaliador | TEXT | Nome do avaliador |
| nota | INTEGER | Nota 1-5 |
| comentario | TEXT | Comentário |
| criado_em | DATETIME | Data da avaliação |

### 3. Template Tally (Form Padrão)

**Campos do formulário:**
1. Nome completo (obrigatório)
2. Email (obrigatório)
3. Telefone/WhatsApp (obrigatório)
4. LinkedIn (opcional)
5. Pretensão salarial (opcional)
6. Disponibilidade para início
7. Como conheceu a vaga
8. Por que quer trabalhar na Handcom?
9. Upload de currículo (PDF)
10. Observações adicionais

**Hidden fields:**
- vaga_id (para identificar a vaga)
- vaga_titulo

### 4. Webhook Receiver

O OpenClaw precisa de um endpoint público para receber webhooks do Tally.

**Opções:**
1. **Tailscale Funnel** - Expõe endpoint local
2. **Cloudflare Worker** - Recebe e encaminha pro Henry via API
3. **Vercel Function** - Mesmo conceito
4. **n8n/Make** - Intermediário visual

**Recomendação:** Cloudflare Worker (grátis, confiável)

### 5. Site de Vagas

**Tecnologia:** Hugo (gerador estático)

**Estrutura:**
```
site/
├── config.toml
├── content/
│   └── vagas/
│       ├── dev-backend.md
│       └── analista-suporte.md
├── layouts/
│   ├── _default/
│   └── vagas/
├── static/
│   └── img/
└── themes/
    └── handcom/
```

**Deploy:** GitHub Actions → GitHub Pages

**Fluxo:**
1. Henry gera arquivos markdown das vagas
2. Commit no repo
3. GitHub Actions builda com Hugo
4. Deploy automático no GitHub Pages

**Domínio sugerido:** vagas.handcom.com.br (CNAME)

---

## 🔌 Integração Tally

### API Endpoints Utilizados

| Endpoint | Método | Uso |
|----------|--------|-----|
| /forms | POST | Criar formulário |
| /forms/{id} | GET | Obter formulário |
| /forms/{id} | PATCH | Atualizar formulário |
| /forms/{id} | DELETE | Deletar formulário |
| /webhooks | POST | Criar webhook |
| /forms/{id}/submissions | GET | Listar submissões |

### Autenticação
```
Authorization: Bearer tly-qy0cD4CTE9TMBf8sehD3xHDhfCDX9DGR
```

---

## 🤖 Triagem com IA

### Critérios de Avaliação

1. **Match de Requisitos** (40%)
   - Skills técnicas mencionadas
   - Experiência relevante
   - Formação

2. **Comunicação** (20%)
   - Clareza do currículo
   - Qualidade das respostas

3. **Fit Cultural** (20%)
   - Motivação demonstrada
   - Alinhamento com valores

4. **Red Flags** (-pontos)
   - Gaps inexplicados
   - Inconsistências
   - Pretensão fora da faixa

### Output da Análise

```json
{
  "score": 78,
  "classificacao": "apto",
  "pontos_fortes": [
    "5+ anos experiência Java",
    "Conhece WMS",
    "Comunicação clara"
  ],
  "pontos_atencao": [
    "Não menciona experiência com mobile",
    "Pretensão 15% acima do teto"
  ],
  "sugestao_proximos_passos": "Agendar entrevista técnica",
  "perguntas_sugeridas": [
    "Como você lidaria com...",
    "Pode detalhar sua experiência em..."
  ]
}
```

---

## 📅 Cronograma Sugerido

| Semana | Entrega |
|--------|---------|
| 1 | Skill básica + DB + criar vagas |
| 2 | Integração Tally (criar forms) |
| 3 | Webhook + receber candidatos |
| 4 | Triagem IA básica |
| 5 | Site estático + deploy |
| 6 | Refinamentos + testes |

---

## 🚧 Decisões Pendentes

1. **Webhook receiver:** Tailscale Funnel ou Cloudflare Worker?
2. **Site:** Subdomínio vagas.handcom.com.br ou /vagas no site principal?
3. **Notificações:** Email automático para candidatos ou manual?
4. **Multi-empresa:** Vagas só da Handcom ou também SmartRetail/GP?

---

## 📝 Notas

- API Key Tally armazenada de forma segura
- Limite API: 100 req/min
- Tally é grátis para formulários ilimitados
- Considerar LGPD para dados de candidatos

---

*Criado em: 2026-02-11*
*Última atualização: 2026-02-11*
