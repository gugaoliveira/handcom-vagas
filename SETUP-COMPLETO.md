# 🚀 Sistema de Vagas Handcom - Setup Completo

**Data:** 13/02/2026  
**Status:** ✅ Totalmente Operacional

---

## 📦 O QUE FOI CONFIGURADO

### 1. ✅ GitHub + GitHub Pages

**Repositório criado:**
- URL: https://github.com/gugaoliveira/handcom-vagas
- Visibilidade: Público
- Branch: `main`

**GitHub Pages ativado:**
- URL do site: https://gugaoliveira.github.io/handcom-vagas/
- Source: `/docs` folder
- Status: 🟢 Online

**Conteúdo hospedado:**
- ✅ Portal de vagas (index.html)
- ✅ Logo da Handcom (horizontal.jpg, icon.jpg)
- ✅ Banners gerados por IA
- ✅ Dados JSON das vagas

---

### 2. ✅ Logos Hospedadas

**URLs públicas:**
```
https://gugaoliveira.github.io/handcom-vagas/assets/horizontal.jpg
https://gugaoliveira.github.io/handcom-vagas/assets/icon.jpg
```

**Como usar no Tally:**
1. Acesse o formulário no Tally
2. Adicione bloco "Image"
3. Cole a URL da logo horizontal
4. Salve

---

### 3. ✅ Sistema de Banners Automáticos (Gemini AI)

**Script criado:**
- Localização: `~/.openclaw/workspace/skills/vagas/scripts/gerar_banner.py`
- Tecnologia: Gemini 3 Pro Image (Nano Banana Pro)
- Resolução: 1920x1080 (2K)
- Estilo: Moderno, tech, profissional

**Características do banner:**
- Logo da Handcom no topo
- Título da vaga em destaque
- Principais tecnologias em badges
- Local e modalidade
- URL do site
- Design otimizado para redes sociais

**Comando integrado no CLI:**
```bash
cd ~/.openclaw/workspace/skills/vagas
./vagas banner <vaga_id>
```

**Exemplo de uso:**
```bash
# Gerar banner para vaga #1
./vagas banner 1

# Output:
# 🎨 Gerando banner para: Desenvolvedor Backend
# ✅ Banner gerado com sucesso!
# 📍 Path: /Users/gustavooliveira/.openclaw/workspace/projects/vagas-handcom/docs/banners/vaga-1-...png
# 🌐 URL pública: https://gugaoliveira.github.io/handcom-vagas/banners/vaga-1-...png
```

**Primeiro banner gerado:**
- Vaga: #1 Desenvolvedor Backend
- Path: `docs/banners/vaga-1-2026-02-13-16-12-19.png`
- URL: https://gugaoliveira.github.io/handcom-vagas/banners/vaga-1-2026-02-13-16-12-19.png

---

## 🎯 FLUXO COMPLETO DE USO

### Criar Nova Vaga com Banner

```bash
# 1. Criar vaga
cd ~/.openclaw/workspace/skills/vagas
./vagas criar "Desenvolvedor Frontend"

# 2. Editar detalhes
./vagas editar 3 descricao "Desenvolvimento de interfaces modernas com React"
./vagas editar 3 requisitos "React, TypeScript, CSS-in-JS, Testes"
./vagas editar 3 modalidade "Híbrido"
./vagas editar 3 local "Juiz de Fora, MG"

# 3. Gerar banner automático
./vagas banner 3

# 4. Fazer commit do banner
cd ~/.openclaw/workspace/projects/vagas-handcom
git add docs/banners/
git commit -m "Add banner: Desenvolvedor Frontend"
git push

# 5. Publicar vaga (cria form Tally)
cd ~/.openclaw/workspace/skills/vagas
./vagas publicar 3
```

**Resultado:**
- ✅ Form Tally criado
- ✅ Banner disponível publicamente
- ✅ Vaga aparece no site
- ✅ Pronto para divulgar

---

## 📊 RECURSOS DISPONÍVEIS

### URLs Importantes

| Recurso | URL |
|---------|-----|
| Site de vagas | https://gugaoliveira.github.io/handcom-vagas/ |
| Repositório | https://github.com/gugaoliveira/handcom-vagas |
| Logo horizontal | https://gugaoliveira.github.io/handcom-vagas/assets/horizontal.jpg |
| Logo ícone | https://gugaoliveira.github.io/handcom-vagas/assets/icon.jpg |

### Forms Tally Ativos

| Vaga | Form |
|------|------|
| #1 Desenvolvedor Backend | https://tally.so/r/KYlvKD |
| #2 Analista de Suporte | https://tally.so/r/VL5bGJ |

---

## 🛠️ COMANDOS ÚTEIS

### Gerenciar Vagas
```bash
cd ~/.openclaw/workspace/skills/vagas

./vagas listar              # Lista vagas ativas
./vagas ver 1               # Ver detalhes da vaga #1
./vagas candidatos 1        # Ver candidatos da vaga #1
./vagas stats               # Estatísticas gerais
```

### Gerar Banners
```bash
./vagas banner 1            # Gera banner para vaga #1
./vagas banner 2 banner-personalizado.png  # Com nome customizado
```

### Atualizar Site
```bash
cd ~/.openclaw/workspace/projects/vagas-handcom

# Após gerar novos banners ou fazer alterações
git add docs/
git commit -m "Update: novos banners e conteúdo"
git push

# GitHub Pages atualiza automaticamente em ~1 minuto
```

### Adicionar Logo no Tally (Manual)
1. Acesse https://tally.so/forms
2. Abra o formulário da vaga
3. Adicione bloco "Image" no topo
4. Cole: `https://gugaoliveira.github.io/handcom-vagas/assets/horizontal.jpg`
5. Ajuste tamanho (recomendado: 600px largura)
6. Salve

---

## 🎨 PERSONALIZANDO BANNERS

### Modificar Estilo do Banner

Edite: `~/.openclaw/workspace/skills/vagas/scripts/gerar_banner.py`

**Opções disponíveis:**

1. **Resolução:**
   - `1K` - 1024x1024
   - `2K` - 1920x1080 (atual)
   - `4K` - 3840x2160

2. **Estilo do prompt:**
   - Modifique a função `montar_prompt()`
   - Altere cores, layout, elementos visuais
   - Ajuste tom (minimalista, vibrante, corporativo, etc.)

3. **Formato do arquivo:**
   - Linha 50: `output_filename = f"vaga-{vaga_id}-{timestamp}.png"`
   - Customize o padrão de nome

---

## 🔄 WORKFLOW RECOMENDADO

### Para Cada Nova Vaga:

1. **Criar** vaga no sistema
2. **Editar** todos os campos necessários
3. **Gerar banner** automático
4. **Commit** banner no GitHub
5. **Publicar** vaga (cria form Tally)
6. **Adicionar logo** no form Tally (manual, 1x)
7. **Divulgar** nas redes sociais usando o banner gerado

### Divulgação:

**LinkedIn:**
- Post com banner gerado
- Link pro form Tally
- Hashtags: #vagas #tech #juizdefora

**Instagram:**
- Story com banner
- Link na bio ou direct

**Site Handcom:**
- Adicionar botão "Trabalhe Conosco"
- Link para https://gugaoliveira.github.io/handcom-vagas/

---

## 📈 PRÓXIMOS PASSOS SUGERIDOS

### Curto Prazo (Esta Semana):
- [ ] Gerar banners para vaga #2 (Analista Suporte)
- [ ] Adicionar logos nos 2 forms Tally existentes
- [ ] Fazer primeiro post de divulgação no LinkedIn
- [ ] Testar recebimento de candidato (simulação)

### Médio Prazo (Próximas 2 Semanas):
- [ ] Configurar domínio customizado (vagas.handcom.com.br)
- [ ] Integrar webhook Tally → OpenClaw
- [ ] Testar triagem IA com candidato real
- [ ] Criar template de email de resposta

### Longo Prazo (Próximo Mês):
- [ ] Dashboard web de gestão
- [ ] Automação de notificações
- [ ] Sistema de pipeline de candidatos
- [ ] Métricas de conversão

---

## 🆘 TROUBLESHOOTING

### Banner não aparece no site
- Verifique se fez commit e push
- Aguarde 1-2 minutos para deploy do GitHub Pages
- Acesse direto a URL do banner

### Erro ao gerar banner
- Verifique se `GEMINI_API_KEY` está configurada
- Execute: `printenv | grep GEMINI`
- Se não aparecer, adicione no `~/.zshrc` ou `~/.bashrc`

### Form Tally não está recebendo candidatos
- Teste o form manualmente
- Verifique se está publicado (não rascunho)
- Confirme que webhook está configurado

### Site não atualiza após push
- Aguarde até 5 minutos
- Verifique Actions do GitHub: https://github.com/gugaoliveira/handcom-vagas/actions
- Limpe cache do navegador

---

## 📞 SUPORTE

Qualquer dúvida ou problema, fale com o Henry:

- "Henry, como gero um banner?"
- "Henry, minha vaga não apareceu no site"
- "Henry, preciso mudar o estilo dos banners"

---

**Sistema criado por:** Henry (OpenClaw AI)  
**Data:** 13/02/2026  
**Versão:** 1.0
