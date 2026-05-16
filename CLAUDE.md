# Frontend App - CLAUDE.md

## Stack Tecnológica
- **Framework:** Next.js v14+ (App Router)
- **Linguagem:** TypeScript
- **Estilização:** Tailwind CSS + Shadcn/ui
- **Runtime:** Node.js
- **Formatação:** `prettier` e `eslint`

## Responsabilidades
- Renderização híbrida (SSR/ISR para candidatos, CSR para recrutadores)
- Interface pública de candidatos
- Painel administrativo de recrutadores
- Gerenciamento de sessão via HttpOnly cookies JWT
- Polling de status via Valkey para feedback em tempo real

## Rotas

### Fluxo Público (SSR/ISR)
- `/vagas` - Listagem de posições abertas
- `/vagas/[id]` - Detalhes da vaga
- `/vagas/[id]/aplicar` - Formulário de inscrição com upload
- `/processo/[candidato_id]/questionario` - Perguntas técnicas dinâmicas
- `/processo/[candidato_id]/guia` - Roteiro de estudos

### Fluxo Privado (CSR)
- `/admin` - Dashboard de recrutadores
- `/admin/vagas/nova` - Criação de vagas
- `/admin/candidatos/[id]` - Dossiê do candidato

## Comandos de Desenvolvimento

### Instalar dependências
```bash
npm install
```

### Rodar em modo de desenvolvimento
```bash
npm run dev
```

### Build para produção
```bash
npm run build
```

### Rodar testes
```bash
npm test
```

### Formatar código
```bash
npm run format  # prettier
npm run lint    # eslint
```

## Regras de Code Style
- Funções entre 4 e 20 linhas
- Arquivos com menos de 500 linhas
- Nomes específicos (evite sufixos genéricos)
- Tipagem estrita (proibido `any`, `unknown`)
- Early returns, máximo 2 níveis de indentação
- Componentes reutilizáveis via Shadcn/ui
- Preservar comentários existentes
- JSDoc em componentes públicos