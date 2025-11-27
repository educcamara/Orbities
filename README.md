 # Orbities
Orbities: the universe of your focus.

## Sobre

Vindo de um contexto universitário, minha equipe e eu víamos a dificuldade que nós e nossos amigos tínhamos em nos mantermos consisntentes nos estudos, sendo muito afetados pela _procrastinação_. Com isso, estudamos as causas e consequencias da procrastinação no contexto de estudantes universitários, a evitar a baixa no desempenho acadêmico.

Dessa forma, **Orbities** foi criado.

Orbities é um aplicativo feito (inicialmente) para iPad, onde, por meio do planejamento automático de sessões de estudo em grupo, esses estudantes conseguem tirar todo o peso de terem que se planejar do zero, e trazer a motivacao de estudar por meio da colaboração em grupo. Nosso app pode:

- Registrar atividades, especificando do que se trata, trazendo um prazo para finalizá-la
- Planejamento automático por meio de modelos de linguagem, onde por meio da analise do titulo e descricao da atividade, ela cria um planejamento de sessoes de estudos, uma com cada subtópico para estudo
- Registro de questões para cada atividade
- Compartilhamento e colaboracao entre amigos para ralizarem e repsonderem as sessoes juntos
- Dinâmica onde uma parte das questões são escolhidas para as pessoas discutirem entre si

Já em desenvolvimento:
- Geração automática de questões dado o assunto
- Atribuição de questões à sessoes específicas

## Tecnologias e Coisas

- Aplicativo feito em **Swift**
- Frontend modelado em **SwiftUI**
- Persistência e sincronia com **CloudKit** para compartilhamento e atualização em tempo real com outros colaboradoers
- Utilização de APIs de LLMs como **Gemini** (Google) e **Foundation Models** (Apple)
- **Combine** para partes mais profundas do paradigma reativo
- Implementação simples de **Swift Macros** para automatizar conversão de tipos para esquemas para API do Gemini

- Arquitetura **Clean + MVVM**
- Programação Orientada a Protocolos (para inversão de dependência)
- Pipeline de **CI/CD** por meio do **Xcode Cloud**
- **GitFlow** + **Conventional Commits** ao trabalhar com git

## Minhas Contribuições

### Arquitetura Clean

Modelagem e implementação (em equipe) da estruturação de arquivos e diretórios do projeto no Xcode, seguindo a arquitetura CLEAN. A motivação principal era ser capaz de modularizar mais facilmente modulos do aplicativo, conseguindo realizar testes separadamente e implementar mais facilmente injecao de dependencia no frontend.

### Pipeline do Xcode Cloud

Implementação de um pipeline simples para lidar com o Build, Test, Archive e colocar no TestFlight toda vez que uma tag com o padrão v* é criado no repositório.

Além disso, lidar com scripts durante o pipeline por meio do diretório de `ci_scripts`.

### CloudKit no contexto da Clean

Implementação do serviço provedor de dados do CloudKit na camada externa do Clean, imlpementando DTOs para a conversão de objetos compatíveis com CloudKit para as Entidades na camada mais interna do aplicativo (Domain Layer). Além disso, implementar serviço de cache para guardar informações de escopo e de registros do CloudKit para não perderem seu estado durante o uso do app.

Não só isso, mas implementar Subscriptions para mudanças dos registros no cloud por meio de CKQuerySubscription e CKDatabaseSubscription, conseguindo lidar tanto com notificações remotas para atualização em tempo real do estado do app, quanto para Push Notifications.

### Gemini API no contexto da Clean

Da mesma forma que o CloudKit, abstração da lógica por meio de DTOs e protocolos (por causa da Clean), e comunicação direta por meio de uma entidade que wrap a URLSession para se comunicar via HTTP.

### Swift Macros

**Motivação:** \
Um módulo do aplicativo necessitava que a API do Gemini retornasse um [output estruturado](https://ai.google.dev/gemini-api/docs/structured-output?example=recipe), onde eu teria que, para todo o tipo com esse comportamento, definir essa estrutura JSON-like

**Solução:** \
Implementei um macro (`@GeminiResponseSchema`) que automaticamente cria uma variável computável que contém um `Dictionary<String, Any>` (Estilo JSON) que descrevia a estrutura do tipo seguindo a documentação oficial do Gemini API.

### Coordenator de Requisições para Performance

**Motivação:** \
Várias telas na hierarquia de fluxo podem pedir o mesmo request, onde a pessoa pode entrar em uma tela e ela executar a mesma requisicao da tela anterior ou outros probelmas de conflitos de reuqisicao

**Solução:** \
Criei o `FetchCoordinator`, entidade que coordena as tarefas em andamento, implementando políticas de concorrência de tarefas como coalesce e force refresh e comunicação com cache (Cache else fetch, stale with revalidate)

### Arquitetura Pub/Sub para comunicação entre Telas

**Motivação:** \
Facilitar a atualizacao e passagem de dados entre as telas da hierarquia do fluxo, sem contar o recebimento de notificacoes de atualizações vindas do CloudKit, para que a tela atualizasse discretamente.

**Solução:** \
Criar um wrapper por cima do `NotificationCenter` que consegue mandar e receber notificações específicas, onde cada uma tem sua definicao por meio do protocolo `EventNotification` que define um payload padrao para aquele tipo de notificacao, trazendo uma consistencia maior do que NotificationCenter.
