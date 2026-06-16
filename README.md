# ConsultaFilme — Versão com Permissão Android

## Equipe

- Ryan Toledo de Oliveira
- Ricardo Gentil
- Daniel Crispino

## Descrição

App Android de consulta de filmes que consome a API pública OMDb. Esta versão evolui o miniapp anterior adicionando a funcionalidade de fotografar capas físicas de filmes diretamente pelo aplicativo, utilizando a permissão de câmera da plataforma Android.

## Relação com a atividade anterior

Na primeira atividade, o app permitia buscar informações de filmes (título, ano e diretor) digitando o nome no campo de texto e consultando a API OMDb. Nesta versão, foi adicionada uma nova seção de câmera que permite ao usuário fotografar a capa física de um filme para guardar uma referência visual junto à busca. Essa funcionalidade exige o uso da permissão `CAMERA`, que não estava presente na versão anterior.

## API utilizada

- **Nome da API:** OMDb API (The Open Movie Database)
- **Endpoint utilizado:** `https://www.omdbapi.com/?t={titulo}&apikey={chave}`
- **Dados exibidos no app:** Título, Ano de lançamento e Diretor do filme

## Permissão Android utilizada

- **Permissão escolhida:** `android.permission.CAMERA`
- **Onde ela foi declarada no Manifest:** `app/src/main/AndroidManifest.xml`, dentro do elemento `<manifest>`, com a tag:
  ```xml
  <uses-permission android:name="android.permission.CAMERA" />
  ```
- **Por que essa permissão é necessária:** O app permite que o usuário fotografe a capa física de um filme (em DVD, Blu-ray ou cartaz) para guardar uma referência visual. Sem acesso à câmera, essa funcionalidade não é possível.

## Como a permissão é solicitada e tratada

O fluxo segue as boas práticas de runtime permission do Android:

1. O usuário clica no botão **"Fotografar capa"**.
2. O app verifica se a permissão `CAMERA` já foi concedida.
3. **Se já tiver permissão:** a câmera é aberta diretamente.
4. **Se for a primeira solicitação:** o sistema Android exibe o diálogo padrão de permissão.
5. **Se o usuário já negou antes:** o app exibe um `AlertDialog` explicando por que a câmera é necessária e dá ao usuário a opção de permitir ou cancelar.
6. **Se o usuário conceder:** a câmera nativa é aberta e a foto tirada é exibida no app.
7. **Se o usuário negar:** o app exibe uma mensagem informativa e continua funcionando normalmente (a busca por título não é afetada).

O app **nunca quebra** ao ter a permissão negada.

## Como testar a permissão

### No emulador ou dispositivo:

1. Abra o app no Android Studio e execute em emulador ou dispositivo físico.
2. Role a tela até a seção **"Fotografar Capa do Filme"**.
3. Toque no botão **"Fotografar capa"**.
4. Na primeira vez, o sistema solicitará a permissão `CAMERA` — escolha **Permitir** ou **Negar** para ver os dois comportamentos.
5. **Caso Permitir:** a câmera abre; após tirar a foto, ela é exibida no app.
6. **Caso Negar:** o app exibe uma mensagem explicativa e continua operando normalmente.

### Testando o diálogo de justificativa:

1. Negue a permissão na primeira solicitação.
2. Toque no botão novamente.
3. O app exibirá um `AlertDialog` com a explicação antes de solicitar novamente.

### Para resetar as permissões (testar do zero):

- Vá em **Configurações → Apps → ConsultaFilme → Permissões → Câmera → Negar**.
- Abra o app novamente e clique em "Fotografar capa".
