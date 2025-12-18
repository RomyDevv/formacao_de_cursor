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

