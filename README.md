# Journey Platform - Documentação Técnica

Documentação técnica completa da plataforma Journey, apresentando diagramas de casos de uso e arquitetura do sistema através de visualizações interativas com Mermaid.js.

## 📋 Sobre o Projeto

Journey é uma plataforma de aprendizado seguro de IA que combina:
- **Validação multi-camadas** para uso seguro de modelos de linguagem
- **Sistema de gamificação** com XP, badges e progressão
- **Integração com IAs** (Claude 3, GPT-4)
- **Execução em sandbox** via Judge0
- **Arquitetura limpa** seguindo princípios de DDD

## 🎯 Diagramas Disponíveis

Esta documentação apresenta 5 fluxos principais da arquitetura:

### 1. **Autenticação e Dashboard**
Fluxo completo de registro, login com JWT e Redis, carregamento do dashboard personalizado.

**Tecnologias:** JWT, Redis, bcrypt, HttpOnly cookies

### 2. **Sistema de Desafios**
Apresentação e filtragem de desafios por nível/categoria, cache e inicialização.

**Tecnologias:** Cache, Filtering, Challenge System

### 3. **Validação e Segurança** ⭐
O coração da plataforma: sistema de 6 camadas de validação de prompts para uso seguro de IAs.

**Tecnologias:** Security Layers, Risk Scoring, Prompt Validation

### 4. **Execução e Avaliação**
Integração com Claude/GPT-4, execução em sandbox Judge0 e avaliação com métricas (PR, DI, CS).

**Tecnologias:** AI Integration, Judge0 Sandbox, Metrics Calculation, Trap Detection

### 5. **Pontuação e Progressão**
Sistema de gamificação completo com XP, multiplicadores, badges, níveis, streaks e leaderboards em tempo real.

**Tecnologias:** Gamification, XP System, Badges, WebSocket, Leaderboards

## 🚀 Como Visualizar

### Opção 1: GitHub Pages (Recomendado)
Acesse a documentação online em:
```
https://gabrielsarteof.github.io/journey-docs
```

### Opção 2: Servidor Local
```bash
# Clone o repositório
git clone https://github.com/gabrielsarteof/journey-docs.git
cd journey-docs

# Inicie um servidor HTTP local
python -m http.server 8080

# Acesse no navegador
http://localhost:8080
```

## 📁 Estrutura do Projeto

```
journey-docs/
├── index.html                      # Página principal com navegação interativa
├── assets/
│   ├── images/                     # Logos e imagens
│   └── styles/                     # Estilos adicionais
└── docs/
    └── architecture/
        ├── detailed/               # Diagramas detalhados (verticais)
        │   ├── 01-authentication-dashboard.md
        │   ├── 02-challenge-system.md
        │   ├── 03-validation-security.md
        │   ├── 04-execution-evaluation.md
        │   └── 05-scoring-progression.md
        └── simplified/             # Diagramas simplificados (apresentação 16:9)
            ├── 01-authentication-simple.md
            ├── 02-challenge-simple.md
            ├── 03-validation-simple.md
            ├── 04-execution-simple.md
            └── 05-gamification-simple.md
```

## 🛠️ Tecnologias Utilizadas

### Frontend da Documentação
- **HTML5** - Estrutura semântica
- **CSS3** - Estilização responsiva
- **JavaScript (ES6+)** - Interatividade e carregamento dinâmico
- **Mermaid.js** - Renderização de diagramas

### Stack da Plataforma Journey
- **Backend:** Node.js, TypeScript, Fastify, Prisma ORM
- **Cache/Queue:** Redis, BullMQ
- **Real-time:** Socket.io
- **AI Providers:** OpenAI (GPT-4), Anthropic (Claude 3)
- **Sandbox:** Judge0
- **Architecture:** Clean Architecture, Domain-Driven Design

## 🎨 Design System

A documentação segue o design system da plataforma Journey:
- **Cor Primary:** `#0f0f0f` (preto)
- **Background:** `#ffffff` (branco)
- **Fonte:** System UI fonts (San Francisco, Segoe UI)
- **Diagramas:** Tema dark com fundo `#121212`

## 📝 Formato dos Diagramas

Todos os diagramas são escritos em **Mermaid.js** e seguem o padrão:
- Fluxogramas verticais (top-down)
- Nomenclatura clara de componentes
- Comentários explicativos em cada arquivo
- Versões detalhadas e simplificadas

## 🤝 Contribuindo

Este é um projeto de TCC, mas sugestões são bem-vindas:

1. Fork o projeto
2. Crie uma branch para sua feature (`git checkout -b feature/MinhaFeature`)
3. Commit suas mudanças (`git commit -m 'feat: adiciona MinhaFeature'`)
4. Push para a branch (`git push origin feature/MinhaFeature`)
5. Abra um Pull Request

## 📄 Licença

Este projeto faz parte do TCC (Trabalho de Conclusão de Curso) e é disponibilizado para fins educacionais.

## 👤 Autor

**Gabriel Barboza Sarte**
- GitHub: [@gabrielsarteof](https://github.com/gabrielsarteof)
- LinkedIn: [Gabriel Barboza Sarte](https://www.linkedin.com/in/gabriel-sarte/)

## 🔗 Links Relacionados

- [Journey Backend](https://github.com/gabrielsarteof/journey-backend) - Repositório principal da plataforma
- [Journey Frontend](https://github.com/gabrielsarteof/journey-frontend) - Interface web da plataforma
- [Mermaid.js Documentation](https://mermaid.js.org/) - Documentação oficial do Mermaid

---

**Desenvolvido como parte do Trabalho de Conclusão de Curso - 2025**
