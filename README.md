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
- Implementação simples de **Swift Macros** para automatizar conversão de tipos para esquemas para API do Gemini

- Arquitetura **CLEAN + MVVM**
- Programação Orientada a Protocolos (para inversão de dependência)
- Pipeline de **CI/CD** por meio do **Xcode Cloud**
- **GitFlow** + **Conventional Commits** ao trabalhar com git

## Minhas Contribuições

### Arquitetura CLEAN

Modelagem e implementação (em equipe) da estruturação de arquivos e diretórios do projeto no Xcode, seguindo a arquitetura CLEAN
