# Fluxo 1: Autenticação e Dashboard

## Descrição
Este diagrama representa o fluxo inicial do usuário na plataforma Journey, incluindo autenticação (registro/login) e acesso ao dashboard personalizado.

## Componentes
- **AuthController**: Controlador de autenticação
- **JWTService**: Gerenciamento de tokens JWT
- **UserRepository**: Acesso aos dados de usuários
- **DashboardService**: Agregação de dados do dashboard

## Diagrama

```mermaid
%%{init: {'theme':'dark', 'themeVariables': { 'primaryColor':'#121212', 'primaryTextColor':'#fff', 'primaryBorderColor':'#121212', 'lineColor':'#c7c4bf', 'secondaryColor':'#121212', 'tertiaryColor':'#121212', 'background':'#121212', 'mainBkg':'#121212', 'secondBkg':'#1a1a1a', 'nodeBorder':'#121212', 'clusterBkg':'#121212', 'clusterBorder':'#121212', 'titleColor':'#fff', 'edgeLabelBackground':'#121212', 'nodeTextColor':'#fff'}}}%%

flowchart TD
    Start([Usuário Inicia])
    Start --> Login{Autenticado?}

    Login -->|Não| AuthChoice{Escolhe}
    Login -->|Sim| Dashboard[GET /dashboard<br/>GetDashboardUseCase]

    AuthChoice -->|Registrar| Register[POST /auth/register<br/>RegisterUseCase]
    AuthChoice -->|Entrar| LoginFlow[POST /auth/login<br/>LoginUseCase]

    Register --> ValidateReg[Validar Email Único<br/>Hash Password bcrypt]
    ValidateReg --> CreateUser[Criar User + Métricas<br/>UserRepository]
    CreateUser --> GenerateTokens[Gerar JWT + Refresh Token<br/>JWTService]

    LoginFlow --> ValidateCreds[Validar Credenciais<br/>Compare bcrypt]
    ValidateCreds -->|Falha| AuthError[401 Unauthorized]
    ValidateCreds -->|Sucesso| GenerateTokens

    AuthError --> Login

    GenerateTokens --> StoreSession[Armazenar em Redis<br/>+ Cookie HttpOnly]
    StoreSession --> Dashboard

    Dashboard --> LoadProfile[Carregar Perfil do Usuário<br/>Level, XP, Streak]
    LoadProfile --> LoadStats[Carregar Estatísticas<br/>Badges, Achievements]
    LoadStats --> LoadChallenge[Carregar Desafio Diário<br/>Baseado em Level]
    LoadChallenge --> RenderDash[Renderizar Dashboard<br/>Status + Progresso]
    RenderDash --> Ready([Dashboard Pronto])

    style Start fill:#121212,stroke:#121212,color:#fff
    style Ready fill:#121212,stroke:#121212,color:#fff
    style Login fill:#121212,stroke:#121212,color:#fff
    style AuthChoice fill:#121212,stroke:#121212,color:#fff
    style Register fill:#121212,stroke:#121212,color:#fff
    style LoginFlow fill:#121212,stroke:#121212,color:#fff
    style ValidateReg fill:#121212,stroke:#121212,color:#fff
    style CreateUser fill:#121212,stroke:#121212,color:#fff
    style GenerateTokens fill:#121212,stroke:#121212,color:#fff
    style ValidateCreds fill:#121212,stroke:#121212,color:#fff
    style AuthError fill:#121212,stroke:#121212,color:#fff
    style StoreSession fill:#121212,stroke:#121212,color:#fff
    style Dashboard fill:#121212,stroke:#121212,color:#fff
    style LoadProfile fill:#121212,stroke:#121212,color:#fff
    style LoadStats fill:#121212,stroke:#121212,color:#fff
    style LoadChallenge fill:#121212,stroke:#121212,color:#fff
    style RenderDash fill:#121212,stroke:#121212,color:#fff
```

## Endpoints Relacionados
- `POST /auth/register` - Registro de novo usuário
- `POST /auth/login` - Login de usuário existente
- `POST /auth/refresh` - Renovar token JWT
- `GET /dashboard` - Obter dashboard personalizado

## Regras de Negócio
1. Email deve ser único no sistema
2. Senha deve ter mínimo 8 caracteres
3. JWT expira em tempo configurável (padrão: 1h)
4. Refresh token armazenado em Redis com TTL
5. Cookies com flags `httpOnly` e `secure` em produção
6. Rate limit: 5 tentativas de login a cada 15 minutos

## Segurança
- Bcrypt com salt 10+ para senhas
- JWT assinado com HS256
- HttpOnly cookies previnem XSS
- Rate limiting previne brute force
- Helmet middleware adiciona security headers
