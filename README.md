# Starfilmes (HTML/JS + Supabase)

Site de streaming simples: catálogo, busca, player (mp4/HLS), login, Minha Lista e painel de administrador.
Sem a configuração do Supabase ele abre em **modo demonstração** (lê `catalog.json`, sem login).

## 1. Criar o backend (grátis)
1. Crie um projeto em https://supabase.com.
2. No **SQL Editor**, rode `supabase/schema.sql` e depois `supabase/seed.sql`.
3. Em **Project Settings → API**, copie a *Project URL* e a *anon public key* para `js/config.js`.
   (Nunca use a `service_role key` no site.)

## 2. Criar a conta ADM (sem senha no código)
1. Supabase → **Authentication → Users → Add user**: informe o e-mail do administrador e **a senha que você escolher** (use uma forte, não `1234567890`).
2. No SQL Editor, promova esse usuário:
   ```sql
   update public.profiles set role = 'admin'
   where id = (select id from auth.users where email = 'SEU_EMAIL_ADM');
   ```
3. Entre no site com esse e-mail: aparece o painel para adicionar/remover títulos. A regra é aplicada no banco (RLS), não só na tela.

## 3. Testar localmente
```
python3 -m http.server 8000   # abra http://localhost:8000
```

## 4. Subir para o GitHub
```
git init
git add .
git commit -m "Starfilmes: primeira versão"
git branch -M main
git remote add origin https://github.com/SEU_USUARIO/starfilmes.git
git push -u origin main
```

## 5. Publicar
- **Cloudflare Pages** ou **Vercel**: conecte o repositório, sem build command e com a pasta raiz como saída.
- Em Supabase → Authentication → URL Configuration, ponha o endereço do site em *Site URL*.

## Conteúdo
Use só obras em domínio público, Creative Commons ou suas. Guarde a licença de cada título (campo `license`).
Para vídeo, use a URL de um `.mp4` ou `.m3u8` que você tenha direito de distribuir (Cloudflare Stream, Bunny, R2 etc.).
