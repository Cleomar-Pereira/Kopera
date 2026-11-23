# Kopera — Biomassa Control (Android + nuvem privada)

Projeto preparado para a Kopera com a logo enviada, login por e-mail/senha, espaço de empresa no Firebase, dados compartilhados em tempo real, convite individual por e-mail, regras de segurança do Firestore, indicadores, exportação CSV/JSON e cache local/offline do Firestore.

## Importante antes de usar

Este pacote é o **projeto-fonte preparado para gerar o APK**. Ele ainda não é um APK pronto: precisa ser ligado ao projeto Firebase da Kopera e compilado no Android Studio. Não publique o repositório com chaves privadas ou dados reais. A configuração Firebase do cliente contém identificadores públicos do projeto; a proteção real é feita pelas regras Firestore incluídas aqui.

O plano gratuito Firebase Spark pode servir para testes e operação pequena, dentro das cotas. Se ultrapassar as cotas, algumas operações podem parar; confira os limites atuais do Firebase. Dados sincronizados na nuvem ficam no projeto Firebase criado pela empresa, não neste ZIP nem no GitHub.

## 1. Criar Firebase da Kopera

1. Entre em https://console.firebase.google.com/ com uma conta Google controlada pela empresa.
2. Clique em **Adicionar projeto** e crie `kopera-biomassa-control` (ou outro nome disponível).
3. No projeto, abra **Authentication > Começar > Sign-in method** e ative **E-mail/senha**.
4. Abra **Firestore Database > Criar banco de dados**. Para produção, escolha região próxima dos usuários (por exemplo, southamerica-east1 se disponível) e modo de produção.
5. Nas configurações do projeto, em **Seus apps**, adicione um app Web `Kopera Biomassa Control` (ícone `</>`). Copie a configuração `firebaseConfig`.
6. Abra `www/firebase-config.js` e substitua todos os valores `COLOQUE...` / `SEU_...` pelos valores copiados.
7. No Firestore, abra **Regras**, substitua o conteúdo pelas regras de `firestore.rules` deste pacote e clique **Publicar**.

## 2. Criar o primeiro acesso da empresa

1. Compile/abra a versão web ou APK.
2. Toque **Criar acesso**, informe nome, e-mail e senha.
3. Depois de criar a conta, aparecerá **Configurar empresa**. Deixe `Kopera` e toque **Criar empresa**.
4. Essa primeira conta será administradora.

## 3. Adicionar funcionários

1. O administrador entra no app e abre **Equipe**.
2. Informa o e-mail exato do funcionário e toca **Gerar convite**.
3. Envia o código gerado ao funcionário por canal privado.
4. O funcionário cria seu acesso com aquele mesmo e-mail, cola o código em **Código de convite da empresa** e toca **Vincular minha conta à empresa**.
5. O funcionário passa a acessar os mesmos recebimentos e fornecedores da Kopera. O código é de uso único.

## 4. Gerar o APK no Windows

Instale Node.js LTS e Android Studio (com Android SDK). Abra o terminal nesta pasta e execute:

```bash
npm install
npx cap add android
npx cap sync android
npx cap open android
```

No Android Studio, aguarde a sincronização do Gradle. Para testar, conecte um Android com depuração USB e use **Run**. Para gerar APK: **Build > Build Bundle(s) / APK(s) > Build APK(s)**. Para distribuir fora da Play Store, gere uma versão assinada em **Build > Generate Signed Bundle / APK > APK** e guarde a chave de assinatura em local seguro.

Sempre que alterar os arquivos em `www`, execute `npx cap sync android` novamente antes de compilar.

## 5. Dados, privacidade e backups

- Login obrigatório. As regras do Firestore só permitem ler dados da empresa a usuários com registro de membro.
- O administrador cria convites individuais; não há cadastro público automático na organização.
- Recebimentos e fornecedores ficam na nuvem Firestore e sincronizam entre aparelhos autenticados.
- O Firestore mantém cache local para uso offline e envia alterações pendentes quando a conexão volta. Para dados críticos, confirme a sincronização antes de desligar/trocar o aparelho.
- Exporte regularmente o backup JSON e o CSV em **Dados** e guarde em local protegido da empresa.
- Proteja a conta Google administradora e não compartilhe códigos de convite publicamente.
- A primeira versão não importa automaticamente o banco SQLite antigo do Google Colab nem os dados locais da PWA anterior. A migração deve ser feita separadamente, validando os registros antes de usar em produção.

## Arquivos principais

- `www/`: aplicativo e logo da Kopera.
- `www/firebase-config.js`: configuração do Firebase (preencher).
- `firestore.rules`: regras de acesso por empresa/membro.
- `firebase.json`: configuração para Firebase Hosting, opcional.
- `capacitor.config.json`: configuração do app Android.
