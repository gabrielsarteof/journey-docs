# 🚀 Guia de Deploy - GitHub Pages

Este guia mostra como publicar os diagramas do Journey no GitHub Pages para uso durante a apresentação do TCC.

## 📋 Pré-requisitos

- Repositório Git criado (público ou privado)
- Conta no GitHub
- Git instalado localmente

## 🎯 Passo a Passo

### 1. Inicializar o repositório (se ainda não fez)

```bash
cd journey-docs
git init
git add .
git commit -m "Initial commit: Diagramas Journey Platform"
```

### 2. Criar repositório no GitHub

1. Acesse https://github.com/new
2. Nome sugerido: `journey-tcc-docs` ou `journey-docs`
3. Deixe **público** (necessário para GitHub Pages gratuito)
4. **NÃO** inicialize com README (já temos os arquivos)
5. Clique em "Create repository"

### 3. Conectar repositório local ao GitHub

```bash
# Substituir 'seugithub' pelo seu usuário do GitHub
git remote add origin https://github.com/seugithub/journey-docs.git
git branch -M main
git push -u origin main
```

### 4. Ativar GitHub Pages

1. No repositório do GitHub, vá em **Settings**
2. No menu lateral, clique em **Pages**
3. Em **Source**, selecione:
   - Branch: `main`
   - Folder: `/ (root)`
4. Clique em **Save**
5. Aguarde 1-2 minutos

### 5. Acessar o site

Seu site estará disponível em:
```
https://seugithub.github.io/journey-docs/
```

**IMPORTANTE:** Substitua `seugithub` pelo seu usuário do GitHub.

## 📱 Para a Apresentação

### Opção 1: Link Curto (Recomendado)

Use um encurtador de URL para facilitar:
- **bit.ly**: https://bitly.com
- **TinyURL**: https://tinyurl.com

Exemplo: `bit.ly/journey-diagrams`

### Opção 2: QR Code

Gere um QR Code do link:
1. Acesse https://www.qr-code-generator.com
2. Cole a URL do seu GitHub Pages
3. Baixe o QR Code
4. Adicione no slide com texto: "Acesse os diagramas detalhados"

## 📊 Usando os Diagramas nos Slides

### Diagramas Simplificados (para slides)

Os diagramas simplificados estão em:
```
journey-docs/docs/architecture/presentation/
```

**Como usar:**
1. Abra o arquivo `.md` no VS Code
2. Use a extensão "Markdown Preview Mermaid Support"
3. Tire um screenshot do diagrama renderizado
4. Insira no PowerPoint/Google Slides

Ou

1. Copie o código Mermaid do diagrama simplificado
2. Cole em https://mermaid.live
3. Exporte como PNG/SVG
4. Insira no slide

### Diagramas Detalhados (para referência)

Os diagramas completos estão em:
```
journey-docs/docs/architecture/detailed/
```

Adicione no slide:
```
📊 Diagramas detalhados: bit.ly/journey-diagrams
```

Ou QR Code com a mesma URL.

## 🔄 Atualizando o site

Sempre que fizer mudanças:

```bash
git add .
git commit -m "Atualização: descrição da mudança"
git push origin main
```

O GitHub Pages atualiza automaticamente em 1-2 minutos.

## ✅ Checklist Final

Antes da apresentação, verifique:

- [ ] GitHub Pages está ativo e acessível
- [ ] Todos os 5 diagramas estão renderizando corretamente
- [ ] Link curto ou QR Code está funcionando
- [ ] Testou o acesso em um dispositivo móvel
- [ ] Tem screenshot dos diagramas simplificados nos slides
- [ ] Link está acessível durante a apresentação (teste a internet)

## 🎨 Personalizações Opcionais

### Alterar cores do tema

Edite `index.html` e modifique as variáveis CSS:

```css
/* Cor principal */
--primary-color: #c7c4bf;

/* Background */
--bg-gradient: linear-gradient(135deg, #1a1a1a 0%, #2d2d2d 100%);
```

### Adicionar logo/foto

No `index.html`, adicione antes do `<h1>`:

```html
<img src="logo.png" alt="Journey Logo" style="max-width: 200px; margin-bottom: 1rem;">
```

E adicione o arquivo `logo.png` no repositório.

## 🆘 Troubleshooting

### Site não está aparecendo

1. Verifique se o repositório é **público**
2. Confirme que GitHub Pages está ativado em Settings > Pages
3. Aguarde 5-10 minutos (primeira ativação pode demorar)
4. Tente acessar em uma aba anônima (limpa cache)

### Diagramas Mermaid não renderizam

Os diagramas estão em arquivos `.md`. Para visualização web:
1. GitHub renderiza Mermaid automaticamente em arquivos `.md`
2. Para HTML standalone, use: https://mermaid.live para exportar

### Link não funciona durante apresentação

**Plano B:**
1. Tenha os diagramas salvos localmente como PNG
2. Ou abra o GitHub Pages em uma aba antes da apresentação
3. Ou use o Mermaid Live Editor como backup

## 📞 Suporte

Se tiver problemas:
1. Verifique a documentação oficial: https://docs.github.com/pages
2. Teste os diagramas em: https://mermaid.live
3. Valide o HTML em: https://validator.w3.org

## 🎓 Dica para o TCC

Durante a apresentação, quando mencionar a arquitetura:

**Script sugerido:**
> "Para facilitar a visualização, preparei diagramas detalhados que podem ser acessados
> através deste link [mostrar QR code]. Aqui no slide, temos a versão simplificada que
> mostra o fluxo principal de [nome do diagrama]."

Isso demonstra:
- ✅ Organização
- ✅ Preparação
- ✅ Preocupação com a experiência da banca
- ✅ Domínio técnico (tem documentação completa)

---

**Criado para o TCC - Journey Platform | 2025**
