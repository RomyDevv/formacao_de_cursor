# FORMAÇÃO DE CURSOR
by Tiago Romão e Miguel Faustino

# 1º SETUP DO NEXT.JS
ii
1. Download do zip do repositório e já terem aberto o folder no Cursor

2. Abrir o terminal e garantir que estão na pasta tasks

3. De seguida colar o seguinte comando para o terminal: "npm install". Vai criar o esssencial para o projeto de NEXT.JS

4. Para correr o programa localmente colar o seguinte comando para o terminal: "npm run dev" e abrir no browser o local host. Provavelmente: http://localhost:3000

# 2º SETUP DO SUPABASE

Criar conta no supabase: https://supabase.com/dashboard/sign-up  

1. Podem criar conta conectando diretamente com o GitHub ou simplesmente através do vosso email. 

2. Caso utilizem o email provavelmente exigirá verificação de conta.

3. Logo de seguida será necessário criar uma organização:
 - Podem configurar o Nome, Tipo e o Plano (free plan);

4. Será sugerido a criação de um novo projeto. Vão configurar:
 - Nome do Projeto;
 - Palavra-passe da Base de Dados (IMPORTANTE QUE SAIBAM);
 - Região (selecionem Europa);

5. No Supabase já com o projeto aberto, carregar em "Project Overview". Andar para baixo até a secção "Project API" e vão encontrar 2 tipos de keys:
 - Project URL
 - API Key anon key

6. Ainda têm de ir buscar a Service Role Key:
 - Project Settings -> API Keys -> Legacy anon, service_role API keys -> service_role secret

7. Ir ao cursor e criar um novo file em task com o nome ".env.local". Nesse file vão colocar as vossas keys do Supabase da seguinte forma:
NEXT_PUBLIC_SUPABASE_URL=(Project URL)
NEXT_PUBLIC_SUPABASE_ANON_KEY=(API Key anon key)
SUPABASE_SERVICE_ROLE_KEY=(service_role secret)


# 3º DESENVOLVIMENTO DA APP

## Passo 1: Criar a Tabela no Supabase

### No Supabase Dashboard:

1. Ir ao **Table Editor** no menu lateral
2. Clicar em **"New Table"**
3. Configurar a tabela:
   - **Name**: `tasks`
   - **Description**: (opcional) "Tabela para armazenar tarefas"

### Prompt para o Supabase AI (SQL Editor):

Copiar e colar no **SQL Editor** do Supabase:

```sql
-- Create tasks table
CREATE TABLE tasks (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  title TEXT NOT NULL,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- Enable Row Level Security (RLS)
ALTER TABLE tasks ENABLE ROW LEVEL SECURITY;

-- Create policy to allow all operations (for development)
CREATE POLICY "Allow all operations on tasks" ON tasks
  FOR ALL
  USING (true)
  WITH CHECK (true);
```

**Ou usar esta prompt no chat do Supabase:**

> Create a table named "tasks" with the following columns: id (UUID, primary key, auto-generated), title (TEXT, not null), and created_at (TIMESTAMP with time zone, default NOW()). Enable Row Level Security and create a policy that allows all operations for development purposes.


## Passo 2: Criar as Funções de Base de Dados

### Prompt para o Cursor:

> Create a file `app/lib/supabase/tasks.ts` with two functions:
> 1. `getTasks()` - async function that fetches all tasks from the "tasks" table, ordered by created_at descending. Return the data and handle errors.
> 2. `addTask(title: string)` - async function that inserts a new task with the provided title into the "tasks" table. Return the created task and handle errors.
> Use the Supabase client from `./client` and TypeScript types.



## Passo 3: Criar a API Route

### Prompt para o Cursor:

> Create an API route at `app/api/tasks/route.ts` with:
> 1. GET handler: calls `getTasks()` from `@/lib/supabase/tasks` and returns the tasks as JSON with status 200, or error with status 500
> 2. POST handler: extracts `title` from request body, calls `addTask(title)` from `@/lib/supabase/tasks`, and returns the created task as JSON with status 201, or error with status 500
> Use Next.js 16 App Router API route handlers with proper TypeScript types and error handling.

---

## Passo 4: Criar o Componente TaskList

### Prompt para o Cursor:

> Create a React component `TaskList.tsx` at `app/components/TaskList.tsx` that:
> - Fetches tasks from the API route `/api/tasks` using fetch on component mount
> - Displays a loading state while fetching
> - Shows an empty state if no tasks exist
> - Renders a list of tasks showing the title and created_at date
> - Uses Tailwind CSS for styling with a clean, modern design
> - Handles errors appropriately
> - Uses TypeScript with proper types
> - Follows React 19 patterns with hooks (useState, useEffect)

---

## Passo 5: Criar o Componente TaskForm

### Prompt para o Cursor:

> Create a React component `TaskForm.tsx` at `app/components/TaskForm.tsx` that:
> - Has a form with an input field for task title
> - Has a submit button
> - On submit, sends a POST request to `/api/tasks` with the title
> - Shows loading state during submission
> - Clears the form after successful submission
> - Handles errors and displays them to the user
> - Uses Tailwind CSS for styling with a modern, clean design
> - Uses TypeScript with proper types
> - Follows React 19 patterns with useState and form handling
> - Prevents default form submission behavior

---

## Passo 6: Integrar os Componentes na Página Principal

### Prompt para o Cursor:

> Update `app/page.tsx` to:
> - Import and render the `TaskForm` component at the top
> - Import and render the `TaskList` component below the form
> - Add proper spacing and layout using Tailwind CSS
> - Ensure the page has a clean, centered layout with max-width container
> - Add a heading "Task Manager" or similar at the top

---

## Passo 7: Adicionar Atualização Automática da Lista

### Prompt para o Cursor:

> Update the `TaskList` component to accept an optional `refreshTrigger` prop or use a callback pattern. Then update `TaskForm` to accept an `onTaskAdded` callback prop that gets called after successfully adding a task. In `app/page.tsx`, implement state management to trigger a refresh of `TaskList` when a new task is added via `TaskForm`. Use React state and useEffect if needed.

**Alternativa mais simples:**

> Update `TaskList` to refetch tasks when a `refreshKey` prop changes. Update `TaskForm` to call an `onSuccess` callback after adding a task. In `app/page.tsx`, use useState to manage a refresh counter that increments when a task is added, passing it as `refreshKey` to `TaskList`.

---

## Passo 8: Testar a Aplicação

1. Certificar que o servidor está a correr: `npm run dev`
2. Abrir http://localhost:3000
3. Testar:
   - Adicionar uma nova tarefa através do formulário
   - Verificar se a tarefa aparece na lista
   - Verificar se a data de criação está correta
   - Verificar se múltiplas tarefas são exibidas corretamente




