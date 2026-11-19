# Introdução passo a passo ao framework mobile Flutter

📖 **Leia o tutorial: [https://stahe.github.io/br-flutter-etape-par-etape-sept-2026/](https://stahe.github.io/br-flutter-etape-par-etape-sept-2026/)**

Este curso ensina como construir um aplicativo **mobile** com o framework [Flutter](https://flutter.dev) 3.44 e a linguagem [Dart](https://dart.dev) 3.12: um aplicativo Android cujas telas são renderizadas **no celular** a partir de dados JSON fornecidos por um servidor. O mesmo código também gera um aplicativo web.

Ele segue o roteiro do curso [Introdução passo a passo ao framework web React](https://stahe.github.io/br-react-etape-par-etape-sept-2026/) (assim como dos cursos de [Vue.js](https://stahe.github.io/br-vuejs-etape-par-etape-sept-2026/) e [Angular](https://stahe.github.io/br-angular-etape-par-etape-sept-2026/)): mesmo servidor, mesmas telas, escritas no estilo Flutter, para um celular.

| Curso React | Curso Flutter |
|---|---|
| um aplicativo web, no navegador | um aplicativo mobile (Android) e web |
| componentes funcionais, JSX, hooks | widgets, um método `build()`, `setState` |
| HTML + CSS (Bootstrap) | o engine do Flutter renderiza cada pixel (Material 3) |
| React Router | go_router |
| Zustand, TanStack Query | provider (`ChangeNotifier`), um pequeno cache |
| i18next | dicionários JSON, `intl`, `flutter_localizations` |
| o navegador armazena o cookie do token | um cliente HTTP que gerencia cookies (no celular) |
| `localStorage` | `shared_preferences` |

O servidor, no entanto, continua o mesmo: é o servidor JSON do aplicativo **RdvMedecins**, já utilizado pelos clientes React, Vue.js e Angular.

## A abordagem: muitos exemplos curtos e, depois, um estudo de caso

O Flutter exige aprender muitos conceitos de uma só vez (uma linguagem, widgets, um ciclo de vida, programação assíncrona): o curso é, portanto, estruturado em torno de **25 exemplos curtos**, cada um focado em um único conceito. Eles formam um único projeto Flutter: basta um comando `flutter pub get`, seguido de `flutter run -t lib/<exemplo>/main.dart` para executar um deles.

| Capítulo | Conteúdo | Exemplos |
|---|---|---|
| Primeiros passos | um projeto Flutter, `StatelessWidget` / `StatefulWidget`, `setState`, a árvore de widgets, eventos, todos os tipos de campo, um reducer (sealed classes, `switch`), validação, `Form` e `FormField`, formatação (`intl`) | 01–09 |
| Widgets | parâmetros, funções como parâmetros (`ValueChanged`), composição (`child`, builders, widgets genéricos), ciclo de vida (`initState`, `didUpdateWidget`, `dispose`), `InheritedWidget`, mixins e animações, diálogo de confirmação (`showDialog`) | 10–16 |
| Roteamento | go_router 16: rotas, parâmetros, `ShellRoute` + `NavigationBar`, carregamento tardio (lazy loading), `redirect`, guards | 17–18 |
| Programação assíncrona e estado compartilhado | `Future`, `Stream`, `async*`, timers, anti-bounce, respostas obsoletas, `FutureBuilder`, `StreamBuilder`, provider, `shared_preferences`, tema escuro | 19–20 |
| Internacionalização | dicionários JSON, configurações, plurais (`Intl.pluralLogic`), datas, valores, calendário traduzido | 21 |
| O servidor: uma caixa-preta | configuração do servidor JSON, sua API, 48 exemplos com `curl`, o que muda para um cliente mobile | – |
| Comunicação com o servidor | o pacote `http`, o endereço do servidor (emulador, celular, web), o cookie do token no celular, uma camada de acesso à API, um padrão model-view (MVVM), erros do servidor associados aos campos | 22–25 |

Cada exemplo é apresentado com seu código completo, comentado linha por linha, e com capturas de tela de sua execução.

## O servidor: uma caixa-preta

O servidor é o servidor NestJS das lições anteriores, cujos controllers retornam **JSON**. Esta lição o trata como uma **caixa-preta**: nós o configuramos, estudamos sua API e o consultamos com `curl`, mas não precisamos ler seu código (que é fornecido e comentado para os curiosos).

- Todos os erros têm o mesmo formato: `{ "statusCode": 409, "cle": "ERRORS.LOGIN_TAKEN", "params": {...}, "champs": {...} }` — **chaves** de tradução, nunca texto simples;
- Autenticação por um token JWT em um cookie `httpOnly`: o navegador o armazena automaticamente; no celular, é o aplicativo que o armazena e o reenvia;
- Um "modo de teste" para o CAPTCHA, que permite consultar a API com `curl` (e gerar capturas de tela por meio de testes automatizados).

## O estudo de caso: o cliente Flutter do RdvMedecins

Um aplicativo completo de **agendamento de consultas em um consultório médico**, com **todos** os seus arquivos (cerca de trinta) listados e comentados.

- **Flutter moderno**: Material 3, Dart 3 (records, filtragem, sealed classes, expressões `switch`, extensões), go_router, provider, uma arquitetura em camadas (dados, estado, UI).
- **Três papéis**: `ADMIN` (gerencia médicos e clientes), `DOCTOR` (agenda e cancela consultas), `USER` (o paciente: agenda consultas para si mesmo, gerencia sua conta).
- **Privacidade**: um paciente nunca recebe os nomes dos outros pacientes — o servidor não os envia.
- **Todo o estado da tela na URL**: `/agenda?idMedecin=1&jour=2026-10-05&reserver=7`; no celular, o botão Voltar fecha a janela de agendamento; na web, F5 e Voltar/Avançar funcionam.
- **Validação no servidor**: os formulários exibem, abaixo de cada campo, os erros retornados pela API; bloqueio otimista, nomes duplicados, login já utilizado etc.
- **Sessão**: persiste entre execuções (o cookie é armazenado no dispositivo), é restaurada na inicialização (`GET /api/auth/moi`), com a expiração tratada em um único ponto (resposta 401).
- **Otimizado para mobile**: menu lateral (drawer) (ou menu fixo em telas grandes), listas em vez de tabelas, gerenciador de senhas, área de erros acima da barra de navegação.
- **Francês / inglês**, incluindo o calendário e os próprios textos do Flutter.
- **Implantação**: o aplicativo Android (APK) e a versão web compilada, servida pelo próprio servidor JSON.

## Conteúdo do repositório

```
exemples_flutter/          os 25 exemplos curtos (um único projeto Flutter)
rdvmedecins-nestjs-json/   o servidor JSON do RdvMedecins (a “caixa-preta”)
rdvmedecins_flutter/       o cliente Flutter do estudo de caso
```

As pastas específicas de cada plataforma (`android/`, `web/`...) são geradas por `flutter create` (veja o curso).

## Tecnologias

Flutter 3.44 · Dart 3.12 · Material 3 · go_router 16 · provider 6 · http 1.5 · shared_preferences 2.5 · flutter_svg 2 · intl · flutter_localizations · flutter_test · no servidor: NestJS 10 · TypeORM · MySQL 8 / MariaDB · Passport JWT · svg-captcha

## Pré-requisitos

- Noções básicas de programação orientada a objetos (Java, C#, TypeScript etc.) e do protocolo HTTP. O curso de [Dart](https://stahe.github.io/dart-sept-2026/) é uma boa preparação, mas os conceitos de Dart utilizados são explicados ao longo dos exemplos.
- O SDK do Flutter, o Visual Studio Code e sua extensão Flutter, o Android Studio (para o SDK do Android e o emulador) ou um celular Android, o Chrome; Node.js e um servidor MySQL (por exemplo, o Laragon no Windows) para o servidor JSON. As instruções de instalação são fornecidas nos apêndices do curso.

## Autor

Este curso, seus exemplos, o cliente Flutter e a adaptação do servidor JSON foram escritos por **Claude**, a IA da [Anthropic](https://www.anthropic.com) (setembro de 2026), a pedido de **Serge Tahé**.
