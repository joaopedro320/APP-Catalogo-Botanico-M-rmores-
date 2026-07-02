# Catálogo Digital — Botânico Mármores

Site estático (1 arquivo HTML) com painel de administração e banco de dados em tempo real (Firebase Firestore).

---

## 1. Subir na Vercel (frontend)

**Opção mais simples — arrastar e soltar:**
1. Acesse https://vercel.com e crie uma conta (pode usar login do GitHub/Google)
2. Clique em **"Add New" → "Project"**
3. Escolha **"Deploy without Git"** (ou similar) e arraste esta pasta inteira (`botanico-catalogo-vercel`)
4. Clique em **Deploy**
5. Pronto — a Vercel te dá um link tipo `https://botanico-catalogo.vercel.app`

**Opção via GitHub (recomendada pra facilitar atualizações futuras):**
1. Crie um repositório novo no GitHub e suba esta pasta
2. Na Vercel: **Add New → Project → Import Git Repository** → selecione o repositório
3. Framework Preset: **Other** (é HTML puro, não precisa de build)
4. Deploy

Depois você pode configurar um domínio próprio (ex: `catalogo.botanicomarmores.com.br`) em **Project → Settings → Domains**.

---

## 2. Criar o banco de dados (Firebase Firestore) — GRÁTIS

Sem isso, o site funciona normalmente, mas cada edição feita no painel admin fica salva **só no navegador de quem editou** (localStorage). Com o Firebase, toda edição aparece pra qualquer pessoa que abrir o link, na hora.

### Passo a passo:

1. Acesse **https://console.firebase.google.com**
2. Clique em **"Adicionar projeto"** → dê um nome (ex: `botanico-catalogo`) → pode desativar o Google Analytics → **Criar projeto**
3. No menu lateral, clique em **"Firestore Database"** → **"Criar banco de dados"**
   - Escolha o modo **produção**
   - Região: `southamerica-east1` (São Paulo) se disponível
4. Vá em **Regras** (aba dentro do Firestore) e substitua o conteúdo por:
   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /catalog/{docId} {
         allow read, write: if true;
       }
     }
   }
   ```
   Clique em **Publicar**.
   > ⚠️ Essas regras liberam leitura/escrita pra qualquer pessoa com o link do banco — ok pra um protótipo interno, mas se quiser mais segurança depois, dá pra adicionar login (posso te ajudar nisso quando precisar).

5. Volte para a **Visão geral do projeto** (ícone de engrenagem → Configurações do projeto)
6. Em **"Seus apps"**, clique no ícone **`</>`** (Web) → dê um apelido (ex: `catalogo-web`) → **Registrar app**
7. Vai aparecer um bloco de código com um objeto `firebaseConfig` parecido com isto:
   ```js
   const firebaseConfig = {
     apiKey: "AIzaSy...",
     authDomain: "botanico-catalogo.firebaseapp.com",
     projectId: "botanico-catalogo",
     storageBucket: "botanico-catalogo.appspot.com",
     messagingSenderId: "123456789",
     appId: "1:123456789:web:abcdef123456"
   };
   ```
8. Abra o arquivo **`index.html`** desta pasta, procure por `const firebaseConfig = {` (perto do topo do `<script>`) e cole os valores que o Firebase te deu no lugar dos campos vazios.
9. Salve o arquivo e suba de novo na Vercel (se usou GitHub, é só dar `git push`; se foi arrastar-e-soltar, repita o upload).

Pronto — a partir daí, qualquer edição feita no painel admin (textos, imagens, categorias) fica salva no Firestore e aparece automaticamente pra todo mundo que tiver o link aberto.

---

## 3. Acessando o painel de administração

- No site publicado, clique no ícone de engrenagem discreto no canto inferior direito
- Código de acesso padrão: **1234**
- Para trocar esse código: no `index.html`, procure por `const ADMIN_PIN = "1234";` e troque o número

---

## 4. Resumo do que dá pra editar no painel

- Nome, categoria, indicação de uso e descrição de cada pedra
- Foto de cada pedra (upload direto do computador)
- Criar, renomear e excluir categorias
- Criar e excluir produtos
- Restaurar o catálogo padrão a qualquer momento

---

## Observações técnicas

- É um site 100% estático (HTML + CSS + JS puro) — não precisa de servidor Node, build step, nem framework.
- As imagens enviadas no admin são convertidas em base64 e salvas dentro do próprio documento do Firestore. Isso funciona bem para catálogos de até algumas dezenas de produtos com fotos otimizadas (recomendo comprimir as imagens antes do upload, idealmente abaixo de 300–400KB cada, pois o Firestore tem limite de 1MB por documento).
- O pedido do carrinho é enviado via link `wa.me` direto para o WhatsApp: **+55 11 94006-7852**.
