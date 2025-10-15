# Fluxo 2: Sistema de Desafios

## Descrição
Este diagrama mostra como os desafios são apresentados ao usuário, desde a seleção até o início da resolução. Inclui o sistema de níveis e categorias.

## Componentes
- **ChallengeController**: Gerencia requisições de desafios
- **StartChallengeUseCase**: Inicia tentativa de desafio
- **ChallengeRepository**: Persistência de desafios
- **ChallengeContextService**: Cache de contexto em Redis

## Diagrama


```mermaid
%%{init: {'theme':'dark', 'themeVariables': { 'primaryColor':'#121212', 'primaryTextColor':'#fff', 'primaryBorderColor':'#121212', 'lineColor':'#c7c4bf', 'secondaryColor':'#121212', 'tertiaryColor':'#121212', 'background':'#121212', 'mainBkg':'#121212', 'secondBkg':'#1a1a1a', 'nodeBorder':'#121212', 'clusterBkg':'#121212', 'clusterBorder':'#121212', 'titleColor':'#fff', 'edgeLabelBackground':'#121212', 'nodeTextColor':'#fff'}}}%%

flowchart TD
    Start([Dashboard Pronto])
    Start --> ViewDaily[Ver Desafio Diário<br/>GET /challenges/daily]
    ViewDaily --> FilterByLevel[Filtrar por Nível Usuário<br/>EASY/MEDIUM/HARD/EXPERT]
    FilterByLevel --> FilterByCategory[Filtrar por Categoria<br/>BACKEND/FRONTEND/FULLSTACK]
    FilterByCategory --> SelectChallenge[Selecionar Desafio<br/>Baseado em histórico]
    SelectChallenge --> LoadDetails[GET /challenges/:id<br/>Carregar Detalhes Completos]
    LoadDetails --> DisplayInfo[Mostrar Info do Desafio<br/>Título, Dificuldade, Tempo, Linguagens, XP]
    DisplayInfo --> UserDecision{Iniciar?}

    UserDecision -->|Não| Browse[Navegar Outros<br/>GET /challenges]
    Browse --> LoadDetails

    UserDecision -->|Sim| StartChallenge[POST /challenges/:id/start<br/>StartChallengeUseCase]
    StartChallenge --> CreateAttempt[Criar ChallengeAttempt<br/>Status: IN_PROGRESS]
    CreateAttempt --> CacheContext[Armazenar Contexto em Redis<br/>validation:challengeId:*]
    CacheContext --> LoadStarterCode[Carregar Starter Code<br/>Se disponível]
    LoadStarterCode --> ShowInstructions[Mostrar Instruções<br/>+ Test Cases]
    ShowInstructions --> Ready([Pronto para Resolver])

    style Start fill:#121212,stroke:#121212,color:#fff
    style Ready fill:#121212,stroke:#121212,color:#fff
    style ViewDaily fill:#121212,stroke:#121212,color:#fff
    style FilterByLevel fill:#121212,stroke:#121212,color:#fff
    style FilterByCategory fill:#121212,stroke:#121212,color:#fff
    style SelectChallenge fill:#121212,stroke:#121212,color:#fff
    style LoadDetails fill:#121212,stroke:#121212,color:#fff
    style DisplayInfo fill:#121212,stroke:#121212,color:#fff
    style UserDecision fill:#121212,stroke:#121212,color:#fff
    style Browse fill:#121212,stroke:#121212,color:#fff
    style StartChallenge fill:#121212,stroke:#121212,color:#fff
    style CreateAttempt fill:#121212,stroke:#121212,color:#fff
    style CacheContext fill:#121212,stroke:#121212,color:#fff
    style LoadStarterCode fill:#121212,stroke:#121212,color:#fff
    style ShowInstructions fill:#121212,stroke:#121212,color:#fff
```

## Endpoints Relacionados
- `GET /challenges/daily` - Obter desafio diário personalizado
- `GET /challenges` - Listar todos os desafios disponíveis
- `GET /challenges/:id` - Obter detalhes de um desafio
- `POST /challenges/:id/start` - Iniciar tentativa de desafio

## Estrutura de um Desafio

### Níveis de Dificuldade
- **EASY**: Conceitos básicos, 10-15 min
- **MEDIUM**: Lógica intermediária, 20-30 min
- **HARD**: Algoritmos complexos, 40-60 min
- **EXPERT**: Desafios avançados, 60+ min

### Categorias
- **BACKEND**: APIs, bancos de dados, autenticação
- **FRONTEND**: UI, componentes, estado
- **FULLSTACK**: Integração completa
- **DEVOPS**: Deploy, CI/CD, containerização
- **MOBILE**: Apps nativos/híbridos
- **DATA**: Análise, processamento, ML

## Regras de Negócio
1. Desafio diário muda a cada 24h
2. Usuário pode ter múltiplas tentativas ativas
3. Contexto do desafio em cache por 24h
4. Starter code opcional para guiar iniciantes
5. Test cases públicos e privados
6. Hints liberados progressivamente
7. Traps (armadilhas) detectadas automaticamente

## Cache Strategy
- **Key Pattern**: `validation:{challengeId}:context`
- **TTL**: 86400s (24h)
- **Conteúdo**: keywords, tech stack, tópicos permitidos
