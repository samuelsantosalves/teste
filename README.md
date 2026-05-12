# ModeLar — Painel de Acessibilidade IoT

> Sistema de mapeamento visual para casas inteligentes com foco em acessibilidade.  
> Desenvolvido para CESAR School · GTI 3º Período · Sprint 1.

---

## Stack

| Camada    | Tecnologia                          |
|-----------|-------------------------------------|
| Frontend  | HTML5 semântico + CSS3 + JS (TSDoc) |
| Backend   | Python 3 + Flask                    |
| Banco     | SQLite (`sala_inteligente.db`)      |
| Ícones    | Tabler Icons CDN                    |
| Fontes    | Playfair Display + Outfit (Google)  |

---

## Como rodar localmente

### 1. Instalar dependências do backend

```bash
pip install flask
```

### 2. Iniciar o servidor

```bash
python backend.py
```

O servidor sobe em `http://localhost:5050`. Para verificar:

```bash
curl http://localhost:5050/api/dashboard
```

### 3. Abrir o painel

Abra o arquivo `modelar_v2.html` diretamente no navegador.

> **Nota:** O frontend faz requisições para `localhost:5050`.  
> O banco `sala_inteligente.db` deve estar na mesma pasta que `backend.py`.

---

## Estrutura do projeto

```
modelar/
├── backend.py               # API Flask (8 endpoints REST)
├── sala_inteligente.db      # Banco SQLite com dados seed
├── modelar_v2.html          # Frontend single-file (HTML + CSS + JS)
└── README.md
```

### Endpoints da API

| Método | Rota                          | Descrição                        |
|--------|-------------------------------|----------------------------------|
| GET    | `/api/dashboard`              | Estatísticas gerais              |
| GET    | `/api/dispositivos`           | Lista dispositivos               |
| PUT    | `/api/dispositivos/:id`       | Atualizar estado/brilho          |
| GET    | `/api/eventos`                | Lista eventos sonoros            |
| GET    | `/api/mapeamentos`            | Lista mapeamentos visuais        |
| POST   | `/api/mapeamentos`            | Criar mapeamento                 |
| DELETE | `/api/mapeamentos/:id`        | Remover mapeamento               |
| POST   | `/api/simular/:id`            | Simular evento sonoro            |
| GET    | `/api/logs`                   | Histórico de alertas             |
| PUT    | `/api/logs/:id/resolver`      | Marcar log como visto            |
| PUT    | `/api/logs/resolver-todos`    | Marcar todos como vistos         |
| GET    | `/api/sensores`               | Lista sensores                   |
| GET    | `/api/comandos`               | Lista comandos offline           |
| POST   | `/api/comandos/executar`      | Executar frase de comando        |
| GET    | `/api/perfis`                 | Lista perfis de acessibilidade   |
| POST   | `/api/perfis`                 | Criar perfil                     |

---

## Arquitetura do frontend (TypeScript-style)

O `modelar_v2.html` é organizado internamente como se fosse um projeto modular:

```
AppService   → Camada de API (fetch + error handling)
State        → Estado global da aplicação
Fmt          → Formatadores (data, prioridade, iniciais)
ToastSystem  → Sistema de notificações nativas
Sections     → Registro de seções com loaders
navigateTo() → Roteador SPA sem biblioteca
```

### Interfaces de dados (JSDoc)

```typescript
interface Device   { id_dispositivo, nome, comodo, online, estado, brilho }
interface Event    { id_evento, descricao, nivel_prioridade }
interface Mapping  { id_map, id_evento, id_dispositivo, cor_alerta, padrao_pisca, ... }
interface Log      { id_log, id_evento, data_hora, status_visto, descricao, ... }
interface Sensor   { id_sensor, tipo_sensor, localizacao }
interface Profile  { id_perfil, nome_usuario, intensidade_luz, cor_preferida }
interface Command  { id_comando, frase_comando, acao_executar }
```

---

## Commitar para o Git

### Primeiro commit (setup)

```bash
git init
git add backend.py sala_inteligente.db modelar_v2.html README.md
git commit -m "feat: initial setup - ModeLar accessibility dashboard"
```

### Fluxo de branches sugerido (GitFlow simplificado)

```bash
# Feature nova
git checkout -b feat/skeleton-loaders
# ... alterações ...
git commit -m "feat: add skeleton loaders for device cards"
git checkout main
git merge feat/skeleton-loaders

# Bugfix
git checkout -b fix/cors-backend
git commit -m "fix: add CORS headers to Flask responses"
git checkout main
git merge fix/cors-backend
```

### Padrão de commits (Conventional Commits)

```
feat:     nova funcionalidade
fix:      correção de bug
style:    alteração visual sem mudança de lógica
refactor: refatoração de código
docs:     alteração de documentação
chore:    tarefas de manutenção (deps, config)
```

### Exemplos de commits para este projeto

```bash
git commit -m "feat: add toast notification system"
git commit -m "feat: skeleton loaders for device grid"
git commit -m "fix: handle API connection errors gracefully"
git commit -m "refactor: extract AppService API layer"
git commit -m "style: apply ModeLar brand tokens (teal + gold)"
git commit -m "docs: add README with Git workflow"
```

---

## Funcionalidades implementadas

- [x] Dashboard com 4 métricas em tempo real
- [x] Skeleton loaders nos cards de dispositivos
- [x] Toast notifications nativo (sem `alert()`)
- [x] Controle de dispositivos (ligar/desligar + brilho)
- [x] Mapeamento visual (criar e deletar)
- [x] Simulação de eventos sonoros com popup de alerta
- [x] Histórico de logs com resolução individual e em massa
- [x] Sensores de entrada com indicador de status online
- [x] Comandos offline por frase
- [x] Perfis de acessibilidade
- [x] Zero `onclick` inline — tudo via `addEventListener`
- [x] Navegação acessível (ARIA, `aria-current`, `focus-visible`)
- [x] Design system com CSS Custom Properties

---

## Acessibilidade (WCAG 2.1 AA)

- Contraste de texto ≥ 4.5:1 em todos os elementos interativos
- `role`, `aria-label` e `aria-live` nos componentes dinâmicos
- Navegação completa por teclado (`Tab`, `Enter`, `Space`)
- `focus-visible` com outline customizado na cor da marca
- `aria-current="page"` no item de navegação ativo
