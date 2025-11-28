<h1 align="center">Orbities – the universe of your focus</h1>

<p align="center">
  <img src="assets/orbities-banner.png" alt="Orbities Banner" width="600">  
</p>

<p align="center">
  <a href="https://apps.apple.com/br/app/orbities/id6753018099?itscg=30200&itsct=apps_box_badge&mttnsubad=6753018099">
    <img src="https://toolbox.marketingtools.apple.com/api/v2/badges/download-on-the-app-store/black/pt-br?releaseDate=1759881600"
         alt="Download on the App Store"
         style="width: 245px; height: 82px; object-fit: contain;" />
  </a>
</p>

<p align="center">
  <a href="https://github.com/educcamara">Eduardo</a> (programação) &nbsp;•&nbsp;
  <a href="https://github.com/thalesaraujods">Thales</a> (programação) &nbsp;•&nbsp;
  <a href="https://github.com/naraarrt">Taynara</a> (design)
</p> 

## Sobre

Vindo de um contexto universitário, nós víamos a dificuldade que nós e nossos amigos tínhamos em nos manter consistentes nos estudos, sendo muito afetados pela procrastinação. Com isso, estudamos as causas e consequências da procrastinação no contexto de estudantes universitários, buscando evitar a queda no desempenho acadêmico.

Com isso, **Orbities** foi criado.

Orbities é um aplicativo feito (inicialmente) para iPad, onde, por meio do planejamento automático de sessões de estudo em grupo, esses estudantes conseguem tirar o peso de ter que se planejar do zero e trazer a motivação de estudar por meio da colaboração em grupo. Nosso app pode:

- Registrar atividades, especificando do que se trata e definindo um prazo para finalizá-las
- Fazer planejamento automático por meio de modelos de linguagem, onde, a partir da análise do título e da descrição da atividade, o app cria um planejamento de sessões de estudo, cada uma focada em um subtópico
- Registrar questões para cada atividade
- Permitir compartilhamento e colaboração entre amigos para realizarem e responderem às sessões juntos
- Propor dinâmicas em que uma parte das questões é escolhida para que as pessoas discutam entre si

Já em desenvolvimento:
- Geração automática de questões dado o assunto
- Atribuição de questões a sessões específicas

## Tecnologias e Coisas

- Aplicativo feito em **Swift**
- Frontend modelado em **SwiftUI**
- Persistência e sincronia com **CloudKit** para compartilhamento e atualização em tempo quase real com outros colaboradores
- Utilização de APIs de LLMs como **Gemini** (Google) e **Foundation Models** (Apple)
- **Combine** para partes que exigem um modelo mais reativo
- Implementação simples de **Swift Macros** para automatizar a conversão de tipos para esquemas usados na API do Gemini

- Arquitetura **Clean + MVVM**
- Programação orientada a protocolos (para inversão de dependência)
- Pipeline de **CI/CD** por meio do **Xcode Cloud**
- **GitFlow** + **Conventional Commits** no fluxo de trabalho com Git

## Minhas Contribuições

### MVVM no SwiftUI

Responsável pela aplicação do padrão **MVVM** na camada de apresentação com SwiftUI, separando Views da lógica de negócio e garantindo que cada tela seja guiada por um ViewModel claro, com estados e efeitos bem definidos.

### Arquitetura Clean

Modelagem e implementação (em equipe) da estruturação de arquivos e diretórios do projeto no Xcode, seguindo a arquitetura Clean. A motivação principal era ser capaz de modularizar com mais facilidade os módulos do aplicativo, conseguir realizar testes separadamente e implementar de forma mais simples a injeção de dependências na camada de frontend.

### Pipeline do Xcode Cloud

Implementação de um pipeline simples no **Xcode Cloud** para lidar com build, test e archive, além de envio ao TestFlight sempre que uma tag com o padrão `v*` é criada no repositório.

Além disso, configuração e manutenção de scripts utilizados durante o pipeline por meio do diretório `ci_scripts`.

### CloudKit no contexto da Clean

Implementação do serviço provedor de dados do **CloudKit** na camada externa da Clean Architecture, incluindo:

- Criação de DTOs para conversão de objetos compatíveis com CloudKit para as entidades da camada mais interna do aplicativo (Domain Layer)
- Implementação de um serviço de cache para guardar informações de escopo e registros do CloudKit, evitando perda de estado durante o uso do app
- Implementação de **Subscriptions** para mudanças nos registros na nuvem por meio de `CKQuerySubscription` e `CKDatabaseSubscription`, permitindo:
  - Lidar com notificações remotas para atualização em tempo (quase) real do estado do app
  - Envio e tratamento de push notifications relacionadas a mudanças nos dados

### Gemini API no contexto da Clean

Da mesma forma que com o CloudKit, foi feita a abstração da lógica por meio de DTOs e protocolos (seguindo os princípios da Clean Architecture), com uma camada de comunicação direta por meio de uma entidade que encapsula a `URLSession` para se comunicar via HTTP com a API do Gemini.

### Swift Macros

**Motivação:**  
Um módulo do aplicativo necessitava que a API do Gemini retornasse um [output estruturado](https://ai.google.dev/gemini-api/docs/structured-output?example=recipe), onde seria necessário, para todo tipo com esse comportamento, definir manualmente uma estrutura no formato JSON-like.

**Solução:**  
Implementei um macro (`@GeminiResponseSchema`) que automaticamente cria uma propriedade computada contendo um `Dictionary<String, Any>` (estilo JSON) que descreve a estrutura do tipo, seguindo a documentação oficial da Gemini API.

### Coordenador de Requisições para Performance

**Motivação:**  
Várias telas na hierarquia de fluxo podem disparar a mesma requisição, o que poderia fazer com que o usuário, ao navegar para uma tela, executasse novamente a mesma chamada da tela anterior, além de gerar conflitos e redundância de requisições.

**Solução:**  
Criei o `FetchCoordinator`, uma entidade que coordena as tarefas em andamento, implementando políticas de concorrência como **coalesce** e **force refresh** e se integrando com o cache (`cache-else-fetch`, `stale-while-revalidate`), evitando requisições desnecessárias.

### Arquitetura Pub/Sub para comunicação entre telas

**Motivação:**  
Facilitar a atualização e a passagem de dados entre as telas na hierarquia de fluxo, além de lidar com o recebimento de notificações de atualizações vindas do CloudKit, para que a tela atualizasse discretamente sem acoplamento direto entre componentes.

**Solução:**  
Criei um wrapper em cima do `NotificationCenter` para enviar e receber notificações específicas, onde cada notificação tem sua definição por meio do protocolo `EventNotification`, que define um payload padrão para aquele tipo de evento. Isso trouxe mais consistência e segurança de tipos em comparação com o uso direto do `NotificationCenter`.
