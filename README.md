# Catálogo Digital — Botânico Mármores

Site estático (1 arquivo HTML) com painel de administração e **banco de dados em tempo real (Supabase) já configurado**.

As edições feitas no painel admin ficam salvas no banco e aparecem **automaticamente para todos** que acessarem o link, de qualquer dispositivo.

---

## 1. Subir na Vercel

**Opção mais simples — arrastar e soltar:**
1. Acesse https://vercel.com e crie/entre na conta
2. Clique em **"Add New" → "Project"**
3. Escolha **"Deploy without Git"** e arraste esta pasta inteira (`botanico-catalogo-vercel`)
4. Clique em **Deploy**
5. A Vercel te dá um link tipo `https://botanico-catalogo.vercel.app`

**Opção via GitHub (recomendada para atualizações futuras):**
1. Suba esta pasta num repositório do GitHub
2. Na Vercel: **Add New → Project → Import Git Repository** → selecione o repositório
3. Framework Preset: **Other** (é HTML puro, sem build)
4. Deploy

Depois dá para apontar um domínio próprio (ex: `catalogo.botanicomarmores.com.br`) em **Project → Settings → Domains**.

---

## 2. Banco de dados — JA ESTA PRONTO

O banco de dados **já foi criado e configurado** no Supabase, e o catálogo com as 24 pedras já está carregado nele. Você **não precisa fazer nada** — o site já vem conectado.

Detalhes (só para referência):
- Projeto Supabase: **botanico-catalogo** (região São Paulo)
- Tabela: `catalog` (guarda o catálogo inteiro em JSON)
- Sincronização em tempo real ativada
- As credenciais públicas já estão embutidas no `index.html`

**Como funciona na prática:**
- Você edita algo no painel admin (texto, foto, categoria, cor) e salva no banco
- Qualquer pessoa com o link aberto vê a mudança na hora, sem recarregar
- Funciona em qualquer PC, celular ou navegador

> A chave usada no site é a **chave pública** (publishable/anon) do Supabase — é feita para ficar no front-end e não dá acesso administrativo ao banco. O acesso ao painel de edição é protegido pelo código PIN.

---

## 3. Painel de administração

- No site publicado, clique no ícone de engrenagem discreto no canto inferior direito
- Código de acesso padrão: **1234**
- Para trocar: no `index.html`, procure por `const ADMIN_PIN = "1234";` e altere

No painel dá para:
- Editar nome, categoria, indicação de uso, cor e descrição de cada pedra
- Trocar a foto de cada pedra (upload do computador)
- Criar, renomear e excluir categorias
- Criar e excluir pedras
- Botão **Salvar** em cada pedra e categoria
- Restaurar o catálogo padrão

---

## Observações técnicas

- Site 100% estático (HTML + CSS + JS puro) + Supabase como backend. Sem servidor próprio, sem build.
- As fotos enviadas no admin viram base64 e são salvas no banco junto do catálogo. Para catálogos maiores, recomenda-se comprimir as imagens antes do upload (idealmente < 300-400KB cada).
- Pedidos do cliente saem via link wa.me direto para o WhatsApp: **+55 11 94006-7852**.
- Se o Supabase ficar indisponível por algum motivo, o site continua funcionando com uma cópia local (localStorage) como reserva.
- Link do site institucional do cliente: https://botanicomarmores.com/
