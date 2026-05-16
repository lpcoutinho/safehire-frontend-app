# Frontend App - Plano de Execução Detalhado

## Visão Geral

O **Frontend App** é a aplicação Next.js híbrida da plataforma SafeHire AI. Responsável por renderizar interfaces públicas para candidatos (SSR/ISR) e painel administrativo para recrutadores (CSR), consumindo APIs de forma segura.

### Propósito
- Renderizar páginas públicas para candidatos (SSR/ISR)
- Renderizar painel administrativo para recrutadores (CSR)
- Gerenciar autenticação via HttpOnly cookies
- Fazer upload de currículos
- Exibir status de processamento em tempo real
- Implementar polling para status updates

### Stack Tecnológica
- **Framework**: Next.js v14+ (App Router)
- **Linguagem**: TypeScript (strict)
- **Estilização**: Tailwind CSS + Shadcn/ui
- **State Management**: React Context + Server Actions
- **HTTP Client**: fetch nativo (next-intl optional)
- **Authentication**: HttpOnly cookies + Middleware
- **Validation**: Zod
- **Testing**: Vitest + Playwright
- **Formatting**: prettier + eslint

---

## Arquitetura do Frontend

```
frontend-app/
├── app/
│   ├── (public)/              # Rotas públicas (candidatos)
│   │   ├── vagas/
│   │   │   ├── page.tsx       # Listagem de vagas (SSR)
│   │   │   └── [id]/
│   │   │       ├── page.tsx   # Detalhes da vaga (ISR)
│   │   │       └── aplicar/
│   │   │           └── page.tsx # Formulário de inscrição
│   │   └── processo/
│   │       └── [candidato_id]/
│   │           ├── questionario/
│   │           │   └── page.tsx # Perguntas técnicas (SSR)
│   │           └── guia/
│   │               └── page.tsx # Guia de estudos (SSR)
│   ├── (admin)/               # Rotas privadas (recrutadores)
│   │   ├── dashboard/
│   │   │   └── page.tsx       # Dashboard (CSR)
│   │   ├── vagas/
│   │   │   ├── nova/
│   │   │   │   └── page.tsx   # Criar vaga
│   │   │   └── [id]/
│   │   │       └── editar/
│   │   │           └── page.tsx # Editar vaga
│   │   └── candidatos/
│   │       ├── page.tsx       # Listagem candidatos
│   │       └── [id]/
│   │           └── page.tsx   # Dossiê do candidato
│   ├── api/                   # API Routes (server actions)
│   │   ├── auth/
│   │   │   ├── login/route.ts
│   │   │   ├── logout/route.ts
│   │   │   └── register/route.ts
│   │   └── upload/
│   │       └── curriculo/route.ts
│   ├── layout.tsx             # Root layout
│   ├── page.tsx               # Home page
│   ├── globals.css            # Global styles
│   └── middleware.ts          # Auth middleware
├── components/
│   ├── ui/                    # Shadcn/ui components
│   │   ├── button.tsx
│   │   ├── card.tsx
│   │   ├── input.tsx
│   │   ├── form.tsx
│   │   ├── table.tsx
│   │   └── ...
│   ├── candidates/            # Candidate-specific components
│   │   ├── vagas-list.tsx
│   │   ├── vaga-card.tsx
│   │   ├── upload-form.tsx
│   │   ├── status-tracker.tsx
│   │   └── guia-estudos.tsx
│   ├── recruiters/            # Recruiter-specific components
│   │   ├── dashboard.tsx
│   │   ├── vaga-form.tsx
│   │   ├── candidatos-table.tsx
│   │   ├── candidato-dossie.tsx
│   │   └── metrics-card.tsx
│   └── shared/                # Shared components
│       ├── header.tsx
│       ├── footer.tsx
│       ├── loading-skeleton.tsx
│       └── error-boundary.tsx
├── lib/
│   ├── api/
│   │   ├── client.ts          # HTTP client wrapper
│   │   ├── auth.ts            # Auth API calls
│   │   ├── vagas.ts           # Vagas API calls
│   │   └── candidatos.ts      # Candidatos API calls
│   ├── hooks/
│   │   ├── use-auth.ts
│   │   ├── use-vagas.ts
│   │   └── use-polling.ts
│   ├── utils/
│   │   ├── cn.ts              # Tailwind merge
│   │   ├── validation.ts      # Zod schemas
│   │   └── formatters.ts
│   └── types/
│       ├── vaga.ts
│       ├── candidato.ts
│       └── auth.ts
├── tests/
│   ├── unit/
│   │   ├── components/
│   │   │   └── vaga-card.test.tsx
│   │   ├── hooks/
│   │   │   └── use-auth.test.ts
│   │   └── utils/
│   │       └── validation.test.ts
│   ├── e2e/
│   │   ├── candidato-flow.spec.ts
│   │   └── recrutador-flow.spec.ts
│   └── setup.ts
├── public/
│   └── images/
├── .env.example
├── .eslintrc.json
├── .prettierrc
├── next.config.js
├── package.json
├── tailwind.config.ts
├── tsconfig.json
├── vitest.config.ts
├── playwright.config.ts
├── README.md
└── CLAUDE.md
```

---

## Roadmap de Implementação

### Fase 1: Configuração Base (Dia 1)
- [ ] Criar projeto Next.js com App Router
- [ ] Configurar TypeScript estrito
- [ ] Instalar e configurar Tailwind CSS
- [ ] Instalar e configurar Shadcn/ui
- [ ] Criar estrutura de pastas
- [ ] Criar `package.json` com dependências
- [ ] Criar `.env.example`

### Fase 2: Configuração e Utils (Dia 1-2)
- [ ] Configurar `next.config.js`
- [ ] Configurar `tailwind.config.ts`
- [ ] Criar `lib/utils/cn.ts` (tailwind merge)
- [ ] Criar `lib/utils/validation.ts` (Zod schemas)
- [ ] Criar `lib/utils/formatters.ts`
- [ ] Criar `lib/types/` (TypeScript types)

### Fase 3: Camada de API Client (Dia 2-3)
- [ ] Implementar `lib/api/client.ts`:
  - Wrapper para fetch com error handling
  - Cookie management
  - Request/response interceptors
- [ ] Implementar `lib/api/auth.ts`:
  - `login()`, `logout()`, `register()`, `refreshToken()`
- [ ] Implementar `lib/api/vagas.ts`:
  - `listarVagas()`, `buscarVaga()`, `criarVaga()`, `atualizarVaga()`
- [ ] Implementar `lib/api/candidatos.ts`:
  - `aplicarVaga()`, `uploadCurriculo()`, `buscarStatus()`, `buscarResultado()`

### Fase 4: Hooks e Context (Dia 3)
- [ ] Implementar `lib/hooks/use-auth.ts`:
  - Auth state management
  - Login/logout functions
- [ ] Implementar `lib/hooks/use-polling.ts`:
  - Polling hook para status updates
- [ ] Implementar `lib/hooks/use-vagas.ts`:
  - Vagas data fetching
- [ ] Criar `providers/auth-provider.tsx`:
  - Auth Context Provider

### Fase 5: UI Components (Dia 3-4)
- [ ] Instalar Shadcn/ui components base:
  - Button, Card, Input, Form, Table, Dialog, Toast
- [ ] Implementar `components/shared/loading-skeleton.tsx`
- [ ] Implementar `components/shared/error-boundary.tsx`
- [ ] Implementar `components/shared/header.tsx`
- [ ] Implementar `components/shared/footer.tsx`

### Fase 6: API Routes (Dia 4)
- [ ] Implementar `app/api/auth/login/route.ts`
- [ ] Implementar `app/api/auth/logout/route.ts`
- [ ] Implementar `app/api/auth/register/route.ts`
- [ ] Implementar `app/api/upload/curriculo/route.ts`
- [ ] Configurar middleware de autenticação

### Fase 7: Páginas Públicas (Dia 4-6)
- [ ] Implementar `app/(public)/vagas/page.tsx`:
  - Listagem de vagas com filtros (SSR)
- [ ] Implementar `app/(public)/vagas/[id]/page.tsx`:
  - Detalhes da vaga (ISR)
- [ ] Implementar `app/(public)/vagas/[id]/aplicar/page.tsx`:
  - Formulário de inscrição com upload
- [ ] Implementar `app/(public)/processo/[candidato_id]/questionario/page.tsx`:
  - Perguntas técnicas dinâmicas (SSR)
- [ ] Implementar `app/(public)/processo/[candidato_id]/guia/page.tsx`:
  - Guia de estudos personalizado (SSR)

### Fase 8: Componentes de Candidatos (Dia 5-6)
- [ ] Implementar `components/candidates/vagas-list.tsx`
- [ ] Implementar `components/candidates/vaga-card.tsx`
- [ ] Implementar `components/candidates/upload-form.tsx`
- [ ] Implementar `components/candidates/status-tracker.tsx`:
  - Polling de status do Valkey
- [ ] Implementar `components/candidates/guia-estudos.tsx`

### Fase 9: Páginas de Admin (Dia 6-8)
- [ ] Implementar `app/(admin)/dashboard/page.tsx`:
  - Dashboard com métricas (CSR)
- [ ] Implementar `app/(admin)/vagas/nova/page.tsx`:
  - Formulário de criação de vaga
- [ ] Implementar `app/(admin)/vagas/[id]/editar/page.tsx`:
  - Formulário de edição de vaga
- [ ] Implementar `app/(admin)/candidatos/page.tsx`:
  - Listagem de candidatos com tabela
- [ ] Implementar `app/(admin)/candidatos/[id]/page.tsx`:
  - Dossiê completo do candidato

### Fase 10: Componentes de Recrutadores (Dia 7-8)
- [ ] Implementar `components/recruiters/dashboard.tsx`
- [ ] Implementar `components/recruiters/vaga-form.tsx`
- [ ] Implementar `components/recruiters/candidatos-table.tsx`
- [ ] Implementar `components/recruiters/candidato-dossie.tsx`:
  - Destaque de gaps
  - Roteiro de entrevista
  - Métricas de risco
- [ ] Implementar `components/recruiters/metrics-card.tsx`

### Fase 11: Home e Layout (Dia 8)
- [ ] Implementar `app/layout.tsx`:
  - Root layout com providers
- [ ] Implementar `app/page.tsx`:
  - Landing page
- [ ] Implementar `app/middleware.ts`:
  - Auth middleware
- [ ] Configurar theme e globals

### Fase 12: Testes (Dia 8-9)
- [ ] Testes unitários de componentes (Vitest)
- [ ] Testes unitários de hooks
- [ ] Testes unitários de utils
- [ ] Testes E2E do fluxo do candidato (Playwright)
- [ ] Testes E2E do fluxo do recrutador (Playwright)
- [ ] Testes de acessibilidade

### Fase 13: Documentação (Dia 9-10)
- [ ] README.md com instruções
- [ ] Documentação de componentes
- [ ] Guia de estilização
- [ ] Diagramas de fluxo

---

## TodoList Detalhada

### Configuração
- [x] Criar projeto Next.js
- [ ] Criar `package.json`:
  ```json
  {
    "name": "safehire-frontend",
    "version": "1.0.0",
    "scripts": {
      "dev": "next dev",
      "build": "next build",
      "start": "next start",
      "lint": "next lint",
      "format": "prettier --write .",
      "test": "vitest",
      "test:e2e": "playwright test"
    },
    "dependencies": {
      "next": "^14.0.0",
      "react": "^18.2.0",
      "react-dom": "^18.2.0",
      "typescript": "^5.0.0",
      "tailwindcss": "^3.3.0",
      "@radix-ui/react-dialog": "^1.0.0",
      "@radix-ui/react-dropdown-menu": "^2.0.0",
      "@radix-ui/react-slot": "^1.0.0",
      "@radix-ui/react-tabs": "^1.0.0",
      "@radix-ui/react-toast": "^1.1.0",
      "class-variance-authority": "^0.7.0",
      "clsx": "^2.0.0",
      "lucide-react": "^0.292.0",
      "tailwind-merge": "^2.0.0",
      "tailwindcss-animate": "^1.0.0",
      "zod": "^3.22.0"
    },
    "devDependencies": {
      "@types/node": "^20.0.0",
      "@types/react": "^18.2.0",
      "@types/react-dom": "^18.2.0",
      "@vitejs/plugin-react": "^4.0.0",
      "@playwright/test": "^1.40.0",
      "eslint": "^8.50.0",
      "eslint-config-next": "^14.0.0",
      "prettier": "^3.0.0",
      "vitest": "^1.0.0",
      "@testing-library/react": "^14.0.0"
    }
  }
  ```
- [ ] Criar `.env.example`:
  ```env
  # API Gateway URL
  NEXT_PUBLIC_API_URL=http://api-gateway:8000
  NEXT_PUBLIC_API_URL_LOCAL=http://localhost:8000

  # App
  NEXT_PUBLIC_APP_NAME=SafeHire AI
  NEXT_PUBLIC_APP_URL=http://localhost:3000

  # Feature Flags
  NEXT_PUBLIC_ENABLE_ANALYTICS=false
  NEXT_PUBLIC_ENABLE_DEBUG=false
  ```
- [ ] Criar `tsconfig.json` (strict):
  ```json
  {
    "compilerOptions": {
      "target": "ES2020",
      "lib": ["dom", "dom.iterable", "esnext"],
      "allowJs": true,
      "skipLibCheck": true,
      "strict": true,
      "noEmit": true,
      "esModuleInterop": true,
      "module": "esnext",
      "moduleResolution": "bundler",
      "resolveJsonModule": true,
      "isolatedModules": true,
      "jsx": "preserve",
      "incremental": true,
      "noUnusedLocals": true,
      "noUnusedParameters": true,
      "noFallthroughCasesInSwitch": true
    },
    "include": ["next-env.d.ts", "**/*.ts", "**/*.tsx"],
    "exclude": ["node_modules"]
  }
  ```
- [ ] Criar `tailwind.config.ts`:
  ```typescript
  import type { Config } from "tailwindcss"

  const config: Config = {
    darkMode: ["class"],
    content: [
      "./pages/**/*.{ts,tsx}",
      "./components/**/*.{ts,tsx}",
      "./app/**/*.{ts,tsx}",
      "./src/**/*.{ts,tsx}",
    ],
    theme: {
      container: {
        center: true,
        padding: "2rem",
        screens: {
          "2xl": "1400px",
        },
      },
      extend: {
        colors: {
          border: "hsl(var(--border))",
          input: "hsl(var(--input))",
          ring: "hsl(var(--ring))",
          background: "hsl(var(--background))",
          foreground: "hsl(var(--foreground))",
          primary: {
            DEFAULT: "hsl(var(--primary))",
            foreground: "hsl(var(--primary-foreground))",
          },
          secondary: {
            DEFAULT: "hsl(var(--secondary))",
            foreground: "hsl(var(--secondary-foreground))",
          },
        },
      },
    },
    plugins: [require("tailwindcss-animate")],
  }
  export default config
  ```

### Types
- [ ] `lib/types/vaga.ts`:
  ```typescript
  export type NivelExperiencia = "junior" | "pleno" | "senior" | "staff"
  export type TipoContrato = "clt" | "pj" | "estagio"

  export interface Vaga {
    id: number
    titulo: string
    descricao: string
    requisitos: string[]
    nivel: NivelExperiencia
    tipo_contrato: TipoContrato
    salario_min?: number
    salario_max?: number
    localizacao: string
    empresa: string
    ativa: boolean
    criada_em: string
    atualizada_em: string
  }

  export interface VagaCreate {
    titulo: string
    descricao: string
    requisitos: string[]
    nivel: NivelExperiencia
    tipo_contrato: TipoContrato
    salario_min?: number
    salario_max?: number
    localizacao: string
    empresa: string
  }

  export interface VagaUpdate {
    titulo?: string
    descricao?: string
    requisitos?: string[]
    nivel?: NivelExperiencia
    tipo_contrato?: TipoContrato
    ativa?: boolean
  }
  ```

- [ ] `lib/types/candidato.ts`:
  ```typescript
  export interface Candidato {
    id: number
    vaga_id: number
    usuario_id: number
    email: string
    nome_completo: string
    s3_key?: string
    status_processo: string
    criado_em: string
  }

  export interface ProcessoStatus {
    candidato_id: number
    status: string
    progress: number
  }

  export interface ResultadoProcesso {
    tentativa_injection: boolean
    justificativa_seguranca: string
    candidato_aprovado_na_triagem: boolean
    desafio_codigo_customizado: string
    roteiro_recrutador: RoteiroPergunta[]
    guia_estudos_candidato: string
  }

  export interface RoteiroPergunta {
    pergunta: string
    resposta_esperada: string
    red_flag: string
  }
  ```

- [ ] `lib/types/auth.ts`:
  ```typescript
  export interface User {
    id: number
    email: string
    nome_completo: string
    role: "recrutador" | "candidato"
  }

  export interface LoginRequest {
    email: string
    password: string
  }

  export interface LoginResponse {
    access_token: string
    refresh_token: string
    token_type: string
    expires_in: number
  }
  ```

### Validation Schemas
- [ ] `lib/utils/validation.ts`:
  ```typescript
  import { z } from "zod"

  export const loginSchema = z.object({
    email: z.string().email("Email inválido"),
    password: z.string().min(8, "Mínimo 8 caracteres"),
  })

  export const registerSchema = z.object({
    email: z.string().email("Email inválido"),
    password: z.string().min(8, "Mínimo 8 caracteres"),
    nome_completo: z.string().min(3, "Mínimo 3 caracteres"),
    role: z.enum(["recrutador", "candidato"]),
  })

  export const vagaCreateSchema = z.object({
    titulo: z.string().min(5, "Mínimo 5 caracteres"),
    descricao: z.string().min(50, "Mínimo 50 caracteres"),
    requisitos: z.array(z.string()).min(3, "Mínimo 3 requisitos"),
    nivel: z.enum(["junior", "pleno", "senior", "staff"]),
    tipo_contrato: z.enum(["clt", "pj", "estagio"]),
    salario_min: z.number().optional(),
    salario_max: z.number().optional(),
    localizacao: z.string().min(1, "Obrigatório"),
    empresa: z.string().min(1, "Obrigatório"),
  })

  export const candidatoCreateSchema = z.object({
    email: z.string().email("Email inválido"),
    nome_completo: z.string().min(3, "Mínimo 3 caracteres"),
    telefone: z.string().min(10, "Mínimo 10 dígitos"),
    linkedin_url: z.string().url().optional().or(z.literal("")),
    github_url: z.string().url().optional().or(z.literal("")),
  })
  ```

### API Client
- [ ] `lib/api/client.ts`:
  ```typescript
  const API_URL = process.env.NEXT_PUBLIC_API_URL || "http://localhost:8000"

  interface ApiResponse<T> {
    data?: T
    error?: string
    status: number
  }

  async function apiRequest<T>(
    endpoint: string,
    options: RequestInit = {}
  ): Promise<ApiResponse<T>> {
    const url = `${API_URL}${endpoint}`

    const response = await fetch(url, {
      ...options,
      headers: {
        "Content-Type": "application/json",
        ...options.headers,
      },
      credentials: "include", // Para HttpOnly cookies
    })

    const data = await response.json().catch(() => null)

    if (!response.ok) {
      return {
        error: data?.detail || "Erro desconhecido",
        status: response.status,
      }
    }

    return { data, status: response.status }
  }

  export const api = {
    get: <T>(endpoint: string) => apiRequest<T>(endpoint),
    post: <T>(endpoint: string, body: unknown) =>
      apiRequest<T>(endpoint, { method: "POST", body: JSON.stringify(body) }),
    put: <T>(endpoint: string, body: unknown) =>
      apiRequest<T>(endpoint, { method: "PUT", body: JSON.stringify(body) }),
    delete: <T>(endpoint: string) =>
      apiRequest<T>(endpoint, { method: "DELETE" }),
  }
  ```

- [ ] `lib/api/vagas.ts`:
  ```typescript
  import { api } from "./client"
  import type { Vaga, VagaCreate, VagaUpdate } from "../types/vaga"

  export async function listarVagas(ativaOnly: boolean = true): Promise<Vaga[]> {
    const response = await api.get<Vaga[]>(`/vagas?ativa_only=${ativaOnly}`)
    return response.data || []
  }

  export async function buscarVaga(id: number): Promise<Vaga | null> {
    const response = await api.get<Vaga>(`/vagas/${id}`)
    return response.data || null
  }

  export async function criarVaga(data: VagaCreate): Promise<Vaga | null> {
    const response = await api.post<Vaga>("/vagas", data)
    return response.data || null
  }

  export async function atualizarVaga(id: number, data: VagaUpdate): Promise<Vaga | null> {
    const response = await api.put<Vaga>(`/vagas/${id}`, data)
    return response.data || null
  }

  export async function deletarVaga(id: number): Promise<boolean> {
    const response = await api.delete(`/vagas/${id}`)
    return response.status === 204
  }
  ```

- [ ] `lib/api/auth.ts`:
  ```typescript
  import { api } from "./client"
  import type { LoginRequest, LoginResponse, User } from "../types/auth"

  export async function login(credentials: LoginRequest): Promise<LoginResponse | null> {
    const response = await api.post<LoginResponse>("/api/auth/login", credentials)
    return response.data || null
  }

  export async function logout(): Promise<void> {
    await api.post("/api/auth/logout", {})
  }

  export async function getUser(): Promise<User | null> {
    const response = await api.get<User>("/api/auth/me")
    return response.data || null
  }
  ```

### Hooks
- [ ] `lib/hooks/use-polling.ts`:
  ```typescript
  import { useEffect, useState, useCallback } from "react"

  interface UsePollingOptions {
    interval?: number
    enabled?: boolean
    onSuccess?: (data: any) => void
  }

  export function usePolling<T>(
    fn: () => Promise<T>,
    options: UsePollingOptions = {}
  ) {
    const { interval = 5000, enabled = true, onSuccess } = options
    const [data, setData] = useState<T | null>(null)
    const [loading, setLoading] = useState(false)
    const [error, setError] = useState<string | null>(null)

    const poll = useCallback(async () => {
      if (!enabled) return

      setLoading(true)
      setError(null)

      try {
        const result = await fn()
        setData(result)
        onSuccess?.(result)
      } catch (e) {
        setError(e instanceof Error ? e.message : "Erro ao buscar dados")
      } finally {
        setLoading(false)
      }
    }, [fn, enabled, onSuccess])

    useEffect(() => {
      poll()

      const intervalId = setInterval(poll, interval)
      return () => clearInterval(intervalId)
    }, [poll, interval])

    return { data, loading, error, poll }
  }
  ```

- [ ] `lib/hooks/use-auth.ts`:
  ```typescript
  import { useState, useEffect } from "react"
  import { login as apiLogin, logout as apiLogout, getUser } from "../api/auth"
  import type { User, LoginRequest } from "../types/auth"

  export function useAuth() {
    const [user, setUser] = useState<User | null>(null)
    const [loading, setLoading] = useState(true)

    useEffect(() => {
      async function loadUser() {
        try {
          const userData = await getUser()
          setUser(userData)
        } catch {
          setUser(null)
        } finally {
          setLoading(false)
        }
      }

      loadUser()
    }, [])

    const login = async (credentials: LoginRequest) => {
      const response = await apiLogin(credentials)
      if (response) {
        const userData = await getUser()
        setUser(userData)
      }
    }

    const logout = async () => {
      await apiLogout()
      setUser(null)
    }

    return { user, loading, login, logout }
  }
  ```

### Components - Candidatos
- [ ] `components/candidates/status-tracker.tsx`:
  ```typescript
  import { usePolling } from "@/lib/hooks/use-polling"
  import { Card } from "@/components/ui/card"
  import { Progress } from "@/components/ui/progress"

  interface StatusTrackerProps {
    candidatoId: number
  }

  export function StatusTracker({ candidatoId }: StatusTrackerProps) {
    const { data: status, loading } = usePolling(
      () => fetch(`/api/candidatos/${candidatoId}/status`).then(r => r.json()),
      { interval: 3000 }
    )

    if (loading || !status) {
      return <Card className="p-6">Carregando status...</Card>
    }

    return (
      <Card className="p-6">
        <div className="space-y-4">
          <div className="flex justify-between items-center">
            <h3 className="font-semibold">Status do Processamento</h3>
            <span className="text-sm text-muted-foreground capitalize">
              {status.status}
            </span>
          </div>
          <Progress value={status.progress} />
          <p className="text-sm text-muted-foreground">
            Progresso: {status.progress}%
          </p>
        </div>
      </Card>
    )
  }
  ```

### Components - Recrutadores
- [ ] `components/recruiters/candidato-dossie.tsx`:
  ```typescript
  import { Card } from "@/components/ui/card"
  import { Badge } from "@/components/ui/badge"
  import type { ResultadoProcesso } from "@/lib/types/candidato"

  interface CandidatoDossieProps {
    resultado: ResultadoProcesso
  }

  export function CandidatoDossie({ resultado }: CandidatoDossieProps) {
    if (resultado.tentativa_injection) {
      return (
        <Card className="p-6 border-red-500">
          <div className="space-y-2">
            <h3 className="text-lg font-semibold text-red-600">⚠️ Alerta de Segurança</h3>
            <p className="text-sm">{resultado.justificativa_seguranca}</p>
          </div>
        </Card>
      )
    }

    return (
      <div className="space-y-6">
        {/* Status de Triagem */}
        <Card className="p-6">
          <div className="flex items-center justify-between">
            <h3 className="font-semibold">Status da Triagem</h3>
            <Badge variant={resultado.candidato_aprovado_na_triagem ? "success" : "destructive"}>
              {resultado.candidato_aprovado_na_triagem ? "Aprovado" : "Não Aprovado"}
            </Badge>
          </div>
        </Card>

        {/* Roteiro de Entrevista */}
        <Card className="p-6">
          <h3 className="font-semibold mb-4">Roteiro de Entrevista</h3>
          <div className="space-y-4">
            {resultado.roteiro_recrutador.map((item, index) => (
              <div key={index} className="border-l-2 border-primary pl-4">
                <p className="font-medium">{item.pergunta}</p>
                <p className="text-sm text-muted-foreground mt-1">
                  Esperado: {item.resposta_esperada}
                </p>
                <p className="text-xs text-red-500 mt-1">Red Flag: {item.red_flag}</p>
              </div>
            ))}
          </div>
        </Card>

        {/* Desafio de Código */}
        <Card className="p-6">
          <h3 className="font-semibold mb-4">Desafio de Código</h3>
          <pre className="bg-muted p-4 rounded-lg text-sm whitespace-pre-wrap">
            {resultado.desafio_codigo_customizado}
          </pre>
        </Card>
      </div>
    )
  }
  ```

### Páginas Públicas
- [ ] `app/(public)/vagas/page.tsx`:
  ```typescript
  import { listarVagas } from "@/lib/api/vagas"
  import { VagasList } from "@/components/candidates/vagas-list"

  // Server-side rendering
  export default async function VagasPage() {
    const vagas = await listarVagas(true)

    return (
      <div className="container mx-auto py-8">
        <h1 className="text-3xl font-bold mb-8">Vagas Disponíveis</h1>
        <VagasList vagas={vagas} />
      </div>
    )
  }
  ```

- [ ] `app/(public)/vagas/[id]/aplicar/page.tsx`:
  ```typescript
  import { buscarVaga } from "@/lib/api/vagas"
  import { UploadForm } from "@/components/candidates/upload-form"
  import { notFound } from "next/navigation"

  // Server-side rendering
  export default async function AplicarPage({ params }: { params: { id: string } }) {
    const vaga = await buscarVaga(parseInt(params.id))

    if (!vaga) {
      notFound()
    }

    return (
      <div className="container mx-auto py-8 max-w-2xl">
        <h1 className="text-3xl font-bold mb-4">Aplicar para {vaga.titulo}</h1>
        <p className="text-muted-foreground mb-8">{vaga.empresa} • {vaga.localizacao}</p>
        <UploadForm vagaId={vaga.id} />
      </div>
    )
  }
  ```

### Páginas de Admin
- [ ] `app/(admin)/dashboard/page.tsx`:
  ```typescript
  import { Dashboard } from "@/components/recruiters/dashboard"

  // Client-side rendering
  export default function AdminDashboardPage() {
    return (
      <div className="container mx-auto py-8">
        <Dashboard />
      </div>
    )
  }
  ```

- [ ] `app/(admin)/candidatos/[id]/page.tsx`:
  ```typescript
  import { CandidatoDossie } from "@/components/recruiters/candidato-dossie"
  import { notFound } from "next/navigation"
  import { buscarResultado } from "@/lib/api/candidatos"

  export default async function CandidatoDossiePage({ params }: { params: { id: string } }) {
    const resultado = await buscarResultado(parseInt(params.id))

    if (!resultado) {
      notFound()
    }

    return (
      <div className="container mx-auto py-8">
        <h1 className="text-3xl font-bold mb-8">Dossiê do Candidato #{params.id}</h1>
        <CandidatoDossie resultado={resultado} />
      </div>
    )
  }
  ```

### Middleware
- [ ] `app/middleware.ts`:
  ```typescript
  import { NextResponse } from "next/server"
  import type { NextRequest } from "next/server"

  const protectedRoutes = ["/admin"]
  const publicRoutes = ["/vagas", "/processo"]

  export async function middleware(request: NextRequest) {
    const { pathname } = request.nextUrl

    // Rotas protegidas requerem auth
    if (protectedRoutes.some(route => pathname.startsWith(route))) {
      const token = request.cookies.get("access_token")

      if (!token) {
        return NextResponse.redirect(new URL("/login", request.url))
      }
    }

    return NextResponse.next()
  }

  export const config = {
    matcher: ["/admin/:path*", "/processo/:path*"],
  }
  ```

---

## Validação e Critérios de Aceitação

### Funcional
- [ ] Candidatos podem visualizar vagas públicas
- [ ] Candidatos podem se aplicar com upload de currículo
- [ ] Status de processamento é atualizado em tempo real
- [ ] Recrutadores podem criar vagas
- [ ] Recrutadores podem ver dossiês de candidatos
- [ ] Autenticação funciona com HttpOnly cookies
- [ ] Rotas protegidas redirecionam para login

### Técnico
- [ ] Páginas públicas usam SSR/ISR
- [ ] Páginas de admin usam CSR
- [ ] Polling não gera vazamento de memória
- [ ] Upload de arquivos funciona corretamente
- [ ] Error handling é consistente

### UX/UI
- [ ] Loading states são mostrados
- [ ] Errors são exibidos claramente
- [ ] Form feedback é imediato
- [ ] Mobile responsive funciona
- [ ] Acessibilidade (WCAG AA)

### Testes
- [ ] Coverage >= 70% (frontend)
- [ ] Testes E2E cobrem fluxos principais
- [ ] Testes de acessibilidade passam

### Código
- [ ] TypeScript strict mode
- [ ] Prettier format
- [ ] ESLint sem warnings
- [ ] Componentes reutilizáveis

---

## Comandos de Desenvolvimento

```bash
# Instalar dependências
npm install

# Rodar em desenvolvimento
npm run dev

# Build para produção
npm run build

# Iniciar produção
npm run start

# Formatar código
npm run format

# Lint
npm run lint

# Rodar testes unitários
npm run test

# Rodar testes E2E
npm run test:e2e
```

---

## Próximos Passos

Após completar este serviço:

1. Integrar com API Gateway
2. Testar autenticação end-to-end
3. Testar upload workflow
4. Otimizar performance (ISR, caching)
5. Configurar analytics

---

## Referências

- `PROJECT_CONTEXT.md` - Arquitetura geral
- `CLAUDE.md` - Regras de código
- `plano-geral-execucao.md` - Roadmap global