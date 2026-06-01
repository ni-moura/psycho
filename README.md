# Mappa Mentis

App de página única (`index.html`) para visualizar e editar um mapa pessoal.
Os dados ficam **criptografados** no repositório (`mapa-mental.json.enc`); a senha
nunca sai do navegador. Hospedado via GitHub Pages.

🔗 **Site:** https://ni-moura.github.io/psycho/

---

## Como atualizar (fluxo do dia a dia)

1. Abra o site → digite a senha → **✏ Entrar no modo edição**.
2. Edite o mapa (criar/mover cards, ligações, observações…).
3. Clique em **☁ Publicar (salvar de verdade)** na toolbar.
4. Preencha **senha de criptografia** + **GitHub token** → **🚀 Cifrar e publicar no GitHub**.
5. Pronto — o `.enc` é cifrado no navegador e commitado direto no repositório.
   O GitHub Pages reconstrói sozinho em ~1 min.

> Depois de publicar pelo site, o clone local fica desatualizado.
> Antes de mexer no `index.html` localmente, rode `git pull`.

---

## O que significa cada botão (grupo "Salvar")

| Botão | O que faz | Persiste? |
|---|---|---|
| **☁ Publicar** | Cifra e commita o `.enc` no GitHub | ✅ Sim — única forma de salvar de verdade |
| **⬇ Exportar cópia (.json)** | Baixa um snapshot local **em texto puro** | Backup manual no seu disco |
| **⬆ Importar (.json)** | Carrega o mapa de um arquivo | — |

⚠️ **Fechar a aba sem Publicar não persiste.** Há um auto-save em `localStorage` por
trás (protege contra reload acidental na mesma sessão), mas no próximo login o app
**sempre recarrega a versão publicada** (o `.enc`). Ou seja: o que não foi publicado se perde.

---

## Modos

- **Edição** — acesso total (criar/editar/apagar, publicar).
- **Leitura** — pode navegar, mover cards e criar/remover ligações, mas **não persiste**
  e não edita conteúdo. Útil para revisar sem risco.

O indicador no topo da toolbar (`👁 leitura · trocar` / `✏ edição · trocar`) **alterna**
o modo ao clicar — mantém o que está na tela.

> O modo é só conveniência de UI, **não** é barreira de segurança: o JS é público.
> A única barreira real é a senha que descriptografa o `.enc`.

---

## GitHub token (para Publicar)

O botão Publicar precisa de um **fine-grained PAT**:

- https://github.com/settings/personal-access-tokens/new
- **Repository access:** Only select repositories → `psycho`
- **Permissions → Contents:** Read and write
- Marque "Salvar neste navegador" no modal se quiser não recolar toda vez
  (fica no `localStorage` — evite em máquina compartilhada).

Quando o token expirar, gere outro e cole de novo no modal.

---

## Criptografia

- **AES-256-GCM**, chave derivada por **PBKDF2-SHA-512 (210.000 iterações)**.
- Tudo roda no navegador via Web Crypto (exige HTTPS / contexto seguro — o Pages atende).
- Formato do `.enc`: `[ salt 16B | IV 12B | ciphertext ]`.
- `criptogramos.html` é uma ferramenta avulsa para cifrar/decifrar arquivos manualmente
  (mesmo formato); não é necessária no fluxo normal.

---

## Arquivos

| Arquivo | Versionado | Conteúdo |
|---|---|---|
| `index.html` | ✅ | O app inteiro (HTML+CSS+JS) |
| `mapa-mental.json.enc` | ✅ | Dados **cifrados** |
| `criptogramos.html` | ✅ | Ferramenta avulsa de cifra/decifra |
| `mapa-mental.json` | ❌ (`.gitignore`) | Dados em **texto puro** — nunca commitar |

> Qualquer `*.json` é ignorado pelo `.gitignore` — só o `.enc` cifrado vai para o repositório.
