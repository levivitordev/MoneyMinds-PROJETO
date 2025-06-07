# MoneyMinds: Educação Financeira para Jovens 🧠💰
---

## 📙 Sobre o Projeto

O **MoneyMinds** foi criado com o objetivo de preencher a lacuna na educação financeira para o público jovem. A aplicação funciona como uma Single Page Application (SPA), onde todas as interações ocorrem dinamicamente em uma única página, proporcionando uma experiência de usuário fluida e rápida, semelhante a um aplicativo móvel.

A plataforma utiliza o **Supabase** como backend, gerenciando a autenticação de usuários (incluindo login com Google), o armazenamento de dados em banco de dados (perfis, transações, metas) e o upload de arquivos (fotos de perfil).

---

## 🚀 Principais Funcionalidades

* ✅ **Autenticação Segura**: Cadastro e login com e-mail/senha e OAuth com Google.
* 👤 **Perfis de Usuário**: Perfis distintos para "Jovem" e "Professor", com avatares personalizáveis.
* 🎯 **Metas Financeiras**: Crie, visualize e acompanhe o progresso de suas metas de poupança.
* 💸 **Controle de Transações**: Adicione e visualize um histórico de gastos e recebimentos.
* 🏆 **Desafios Gamificados**: Participe ou crie desafios para engajar e ensinar na prática.
* 📈 **Simulador de Investimentos**: Compare o rendimento potencial entre poupança e ações.
* 📱 **Design Responsivo**: Interface limpa e moderna, construída com TailwindCSS.
* 🔄 **Navegação Dinâmica**: Experiência de SPA sem recarregamentos de página.

---

## 🛠️ Tecnologias Utilizadas

* **Frontend**:
    * HTML5
    * CSS3
    * [TailwindCSS](https://tailwindcss.com/): Framework CSS para estilização rápida.
    * JavaScript (ES6+): Lógica principal da aplicação.
    * [Feather Icons](https://feathericons.com/): Biblioteca de ícones.

* **Backend (BaaS - Backend as a Service)**:
    * [Supabase](https://supabase.io/): Alternativa open-source ao Firebase.
        * **Authentication**: Gerencia o login e registro de usuários.
        * **Database**: Banco de dados PostgreSQL para armazenar os dados.
        * **Storage**: Armazena os avatares dos usuários.

* **Hospedagem**:
   * [Vercel](https://vercel.com/)

---

## 📂 Estrutura do Projeto

O projeto é composto por três arquivos principais:

* `index.html`: Contém toda a estrutura HTML da aplicação. Cada "tela" (login, home, metas, etc.) é uma `div` com a classe `.screen`. A navegação consiste em mostrar ou esconder essas `divs`.
* `style.css`: Inclui estilos personalizados que complementam o TailwindCSS, como animações, a barra de progresso circular e o estilo dos modais.
* `script.js`: O cérebro da aplicação. Contém todo o código JavaScript para manipulação do DOM, navegação entre telas, comunicação com o Supabase e lógica de negócios.
*  `config.js`: Estabelece a Conexão do SupaBase com o Vercel.
  
---

## 👨‍💻 Documentação do Código (`script.js`)

O arquivo `script.js` está organizado em seções lógicas para facilitar o entendimento.

### Navegação e Estado Global
O estado da aplicação é mantido em variáveis globais, e a navegação é gerenciada por um sistema simples de histórico.

* `currentUserType`, `currentUserName`: Armazenam informações do usuário logado.
* `navigationHistory`: Um array que funciona como uma pilha para o histórico de telas visitadas.
* `MapsTo(screenId, params)`: A função central de navegação. Esconde todas as telas e exibe apenas a tela com o `screenId` correspondente. Também carrega dados específicos da tela quando necessário.
* `goBack()`: Remove a última tela do `navigationHistory` e navega para a tela anterior.

### Autenticação de Usuários
As funções de autenticação interagem com o `supabase.auth`.

* `window.onload`: Contém o listener `onAuthStateChange`, que detecta eventos de `SIGNED_IN` e `SIGNED_OUT`, direcionando o usuário para a tela apropriada (home ou login). Ele também trata a criação de um perfil padrão para novos usuários via login social.
* `login()`: Autenticação com e-mail e senha.
* `signInWithGoogle()`: Inicia o fluxo de login com OAuth do Google.
* `cadastrar()`: Registra um novo usuário e, em seguida, cria uma entrada correspondente na tabela `perfis`.
* `logout()`: Desconecta o usuário, limpa os dados da interface e retorna para a tela de login.

### Gerenciamento de Dados
Estas funções são responsáveis por criar, ler, atualizar e deletar (CRUD) dados nas tabelas do Supabase.

* **Transações**:
    * `loadTransactions()`: Busca e exibe a lista completa de transações do usuário na tela de transações.
    * `addNovaTransacao()`: Insere um novo registro na tabela `transacoes`.
* **Metas**:
    * `loadMetas()`: Busca e exibe todas as metas do usuário na tela de metas.
    * `addNovaMeta()`: Insere um novo registro na tabela `metas`.
* **Home**:
    * `loadHomeData()`: Função agregadora que é chamada ao carregar a home do jovem. Ela invoca outras funções para carregar o saldo, a meta ativa e as transações recentes.
    * `calculateAndDisplaySaldo()`: Calcula o saldo total somando recebimentos e subtraindo gastos.
* **Perfil**:
    * `uploadAvatar()`: Gerencia o upload de uma nova imagem de perfil para o Supabase Storage e atualiza a URL na tabela `perfis`.

### Componentes de UI
Funções que controlam elementos específicos da interface, como modais e abas.

* `openModal(modalId)` / `closeModal(modalId)`: Funções genéricas para exibir ou ocultar modais.
* `showSuccessModal(title, message)`: Exibe um modal de sucesso padronizado.
* `switchDesafioTab(tabName)`: Alterna entre as abas "Ativos" e "Concluídos" na tela de desafios.
* `simularInvestimento()`: Realiza o cálculo simples de juros compostos para o simulador.
