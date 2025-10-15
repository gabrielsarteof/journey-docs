# Diagramas de Casos de Uso - Journey Platform

## Visão Geral

Este diretório contém os diagramas de casos de uso da plataforma Journey, divididos em fluxos independentes e otimizados para apresentações em formato 16:9. Todos os diagramas foram criados com Mermaid.js e seguem a arquitetura implementada no `journey-backend`.

## Estrutura dos Diagramas

Os diagramas foram divididos em 5 fluxos principais para facilitar a visualização:

### 1. Autenticação e Dashboard
**Arquivo**: `01-authentication-dashboard.md`

**Fluxo**:
- Registro de novo usuário
- Login de usuário existente
- Geração de JWT e sessão
- Carregamento do dashboard personalizado

**Componentes principais**:
- AuthController
- JWTService
- UserRepository
- DashboardService

**Endpoints**:
- `POST /auth/register`
- `POST /auth/login`
- `GET /dashboard`

---

### 2. Sistema de Desafios
**Arquivo**: `02-challenge-system.md`

**Fluxo**:
- Visualização do desafio diário
- Filtragem por nível e categoria
- Início de tentativa de desafio
- Carregamento de contexto e starter code

**Componentes principais**:
- ChallengeController
- StartChallengeUseCase
- ChallengeRepository
- ChallengeContextService

**Endpoints**:
- `GET /challenges/daily`
- `GET /challenges/:id`
- `POST /challenges/:id/start`

**Níveis**: EASY, MEDIUM, HARD, EXPERT

**Categorias**: BACKEND, FRONTEND, FULLSTACK, DEVOPS, MOBILE, DATA

---

### 3. Validação e Segurança
**Arquivo**: `03-validation-security.md`

**Fluxo**: Sistema multi-camadas de validação de prompts (coração da plataforma)

**6 Camadas de Validação**:
1. **Detecção de Solicitação Direta** (Risk: 80)
   - Bloqueia pedidos de solução completa

2. **Verificação de Relevância Contextual** (Risk: 40)
   - Valida se prompt é relevante ao desafio

3. **Detecção de Padrões Proibidos** (Risk: 25)
   - Regex customizados por desafio

4. **Detecção de Engenharia Social** (Risk: 90)
   - Bloqueia tentativas de manipulação da IA

5. **Off-Topic Detection** (Risk: 50)
   - Detecta assuntos fora de contexto

6. **Análise de Complexidade** (Risk: 20)
   - Verifica anomalias de complexidade

**Sistema de Scoring**:
- 0-49: SAFE (ALLOW) ✅
- 50-79: WARNING (THROTTLE) ⚠️
- 80-100: BLOCKED (BLOCK) ❌

**Componentes principais**:
- PromptValidatorService (732 linhas)
- SemanticAnalyzerService (opcional, com GPT-4)
- RateLimiterService

**Endpoints**:
- `POST /ai/validate`
- `POST /ai/chat`

---

### 4. Execução e Avaliação
**Arquivo**: `04-execution-evaluation.md`

**Fluxo**:
- Seleção de provider de IA (Claude ou GPT-4)
- Interação com IA para ajuda
- Submissão de código
- Execução em sandbox (Judge0)
- Cálculo de métricas
- Detecção de traps (armadilhas)

**Providers Suportados**:
- **OpenAI**: GPT-4, GPT-4-turbo, GPT-3.5-turbo
- **Anthropic**: Claude 3 (Opus, Sonnet, Haiku)

**Métricas de Avaliação**:
- **Pass Rate (PR)**: % de testes aprovados (peso: 40%)
- **Dependency Index (DI)**: Nível de independência (peso: 40%)
- **Checklist Score (CS)**: Qualidade do código (peso: 20%)

**Fórmula**:
```
Weighted Score = (100 - DI) × 0.4 + PR × 0.4 + (CS × 10) × 0.2
```

**Componentes principais**:
- SubmitSolutionUseCase
- Judge0Service (sandbox Docker)
- AnalyzeCodeUseCase
- TrapDetectorService
- ProviderFactory

**Endpoints**:
- `POST /ai/chat`
- `POST /challenges/:id/submit`

---

### 5. Pontuação e Progressão
**Arquivo**: `05-scoring-progression.md`

**Fluxo**:
- Cálculo de XP com 5 multiplicadores
- Sistema de níveis (1-100)
- Sistema de badges (4 raridades)
- Sistema de streak (dias consecutivos)
- Leaderboards
- Notificações em tempo real via WebSocket

**Fórmula de XP**:
```typescript
XP Final = Base XP (100)
  × Difficulty Multiplier (1.0 - 3.0)
  × Performance Bonus (0.85 - 1.5)
  × First Try Bonus (1.0 - 1.25)
  × Independence Bonus (0.75 - 1.5)
  × Streak Bonus (1.0 - 1.5)
```

**Sistema de Badges**:
- **COMMON** (1/10): Conquistas básicas
- **RARE** (1/50): Conquistas especiais
- **EPIC** (1/100): Conquistas raras
- **LEGENDARY** (1/500): Conquistas ultra raras

**Componentes principais**:
- XPCalculatorService
- AwardXPUseCase
- UnlockBadgeUseCase
- StreakManagerService
- WebSocketServer

**Endpoints**:
- `POST /gamification/award-xp`
- `POST /gamification/unlock-badge`
- `GET /gamification/leaderboard`
- `GET /gamification/dashboard`

**WebSocket Events**:
- `xp:awarded`
- `level:up`
- `badge:unlocked`
- `streak:updated`
- `leaderboard:updated`

---

## Fluxo Completo da Plataforma

```
1. Usuário → Autentica (Diagrama 1)
           ↓
2. Usuário → Recebe Desafio Diário (Diagrama 2)
           ↓
3. Usuário → Escreve Prompt → Validação Multi-Camadas (Diagrama 3)
           ↓
4. Prompt Aprovado → IA Responde → Código Submetido → Avaliação (Diagrama 4)
           ↓
5. Desafio Completo → Cálculo XP → Level Up → Badges → Streak (Diagrama 5)
           ↓
       (Volta para 2)
```

---

## Tecnologias Utilizadas

### Backend
- **Runtime**: Node.js + TypeScript
- **Framework**: Fastify 5.5.0
- **Database**: PostgreSQL + Prisma ORM 6.14.0
- **Cache**: Redis (ioredis)
- **Queue**: BullMQ 5.58.0
- **WebSocket**: Socket.io 4.8.1
- **Logging**: Pino 9.9.0
- **Validation**: Zod 3.25.76

### AI Providers
- **OpenAI SDK**: GPT-4, GPT-3.5, Embeddings
- **Anthropic SDK**: Claude 3 family

### Code Execution
- **Judge0**: Sandbox execution em Docker
- **Suporte**: 60+ linguagens de programação

---

## Arquitetura

Todos os módulos seguem **Clean Architecture** com 3 camadas:

### Domain Layer
- Entities
- Schemas (Zod)
- Services (business logic)
- Repositories (interfaces)

### Application Layer
- Use Cases (casos de uso)
- DTOs

### Infrastructure Layer
- Repositories (implementação)
- Services (implementação)
- Providers (IA, cache, etc)

### Presentation Layer
- Controllers
- Routes
- WebSocket Gateways

---

## Segurança

### Validação de Prompts
- 6 camadas de validação
- Detecção de injection attacks
- Prevenção de engenharia social
- Rate limiting configurável
- Auditoria completa em `ValidationLog`

### Execução de Código
- Sandbox Docker isolado
- Sem acesso à rede
- Limite de recursos (CPU, RAM, tempo)
- Validação de código malicioso
- Timeout automático

### Autenticação
- JWT com HS256
- Bcrypt para senhas (salt 10+)
- HttpOnly cookies
- Rate limiting de login
- Helmet security headers

---

## Métricas e Auditoria

### Métricas Rastreadas
- Pass Rate (PR): Taxa de aprovação em testes
- Dependency Index (DI): Nível de independência
- Checklist Score (CS): Qualidade do código
- Usage Tracking: Tokens e custo de IA
- Validation Logs: Histórico de validações

### Auditoria
- **XPTransaction**: Registro de cada mudança de XP
- **ValidationLog**: Histórico de validações de prompts
- **ChallengeAttempt**: Tentativas de desafios
- **MetricSnapshot**: Métricas de cada tentativa

### Compliance
- LGPD: Dados sensíveis protegidos
- Auditoria: Logs estruturados com Pino
- Rastreabilidade: Request ID em todas operações

---

## Como Usar os Diagramas

### Em Apresentações
1. Cada diagrama foi otimizado para formato 16:9
2. Cores adaptadas (#121212 background, texto branco)
3. Sem bordas para melhor visualização
4. Fluxos divididos para facilitar compreensão

### Para Renderizar
Os diagramas Mermaid.js podem ser renderizados em:
- **GitHub**: Suporte nativo em markdown
- **VS Code**: Extensão "Markdown Preview Mermaid Support"
- **Mermaid Live Editor**: https://mermaid.live
- **Apresentações**: Reveal.js, Slidev, etc.

### Para Editar
1. Abra o arquivo `.md` desejado
2. Edite o bloco de código ```mermaid
3. Visualize com ferramenta Mermaid
4. Mantenha o theme dark configurado

---

## Diferenciais da Plataforma Journey

### 1. Validação Inteligente
Sistema multi-camadas único que garante uso educativo de IAs, prevenindo:
- Solicitação direta de soluções
- Engenharia social
- Prompts fora de contexto
- Injection attacks

### 2. Métricas Educativas
- **Dependency Index**: Incentiva independência
- **Pass Rate**: Mede efetividade
- **Checklist Score**: Promove boas práticas
- **Trap Detection**: Corrige conceitos errados

### 3. Gamificação Completa
- XP com 5 multiplicadores diferentes
- Badges com 4 níveis de raridade
- Sistema de streak com bônus
- Leaderboards múltiplos
- Notificações em tempo real

### 4. Segurança em Primeiro Lugar
- Execução isolada em sandbox
- Validação em 6 camadas
- Rate limiting configurável
- Auditoria completa
- Compliance LGPD

### 5. Escalabilidade
- Cache Redis estratégico
- Queue system com BullMQ
- WebSocket para real-time
- Clean Architecture
- Prisma ORM type-safe

---

## Próximos Passos

### Melhorias Futuras
- [ ] Machine Learning para detecção de prompts
- [ ] Mais providers de IA (Gemini, Cohere)
- [ ] Desafios colaborativos (pair programming)
- [ ] Certificações oficiais
- [ ] Mobile apps (React Native)
- [ ] Integração com LMS corporativos
- [ ] Analytics dashboard para gestores
- [ ] Marketplace de desafios customizados

---

## Referências

### Código-fonte
- **Backend**: `journey-backend/`
- **Validação**: `src/modules/ai/infrastructure/services/prompt-validator.service.ts`
- **Gamificação**: `src/modules/gamification/`
- **Challenges**: `src/modules/challenges/`

### Documentação Técnica
- **Prisma Schema**: `journey-backend/prisma/schema.prisma`
- **Environment Config**: `journey-backend/src/config/env.ts`
- **Package Dependencies**: `journey-backend/package.json`

### Arquitetura
- Clean Architecture por módulo
- Domain-Driven Design
- SOLID principles
- Repository pattern
- Factory pattern (AI providers)

---

## Contato e Suporte

Para dúvidas sobre os diagramas ou arquitetura:
- Verifique o código em `journey-backend/`
- Consulte a documentação técnica completa
- Revise os testes em `journey-backend/tests/`

---

**Última atualização**: 2025-10-15
**Versão**: 1.0.0
**Autor**: Journey Platform Team
