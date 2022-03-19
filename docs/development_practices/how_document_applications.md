# Como documentar aplicações

!!! warning "Atenção!"

    Este documento apenas descreve como *eu* documento minhas aplicações.

    Existem diversas estratégias e modelos disponíveis, é claro.
    Mas, esse padrão é o que tem se encaixado melhor até o momento (logo, é muito provável que o padrão apresentado aqui evolua com o tempo).


Ao documentar um software, precisamos ter em mente principalmente quem é o público-alvo. Por exemplo, é uma documentação para:

- Usuário?
- Desenvolvedor?
- Stakeholder?
- Todos?

Podemos dividir os tipos de documentação em:

- **Projeto de Sistema de Software.** Documentação onde são abordados os problemas que o software busca resolver, a(s) solução(ões) propostas, diagrama explicado do sistema tanto alto-nível (componentes em uma visão mais geral) e baixo nível (i.e. diagrama de classes, etc.)
    - **Público-alvo.** Desenvolvedores e stakeholders.
- **Documentação de Uso**. Documentação onde são apresentadas as funcionalidades da ferramenta, assim como utilizar cada uma dessas ferramentas. Aqui, também podem ser incluidas seções sobre idealização da ferramenta, problemas que a aplicação busca solucionar, etc.
    - **Público-alvo.** Usuários, desenvolvedores e stakeholders.

## Projeto de Sistema de Software

Uma documentação de projeto de sistema de software tipo deve ter as seguintes seções:

- **Introdução.** Apresentação do problema a ser resolvido (incluindo o porquê de ser um problema.)
- **Proposta.** Apresentação da proposta/aplicação a ser desenvolvida, incluindo como ela resolve os problemas definidos na Introdução, funcionalidades e qualidades em geral.
- **Arquitetura do Sistema (Alto Nível).** Diagrama do sistema em uma visão de alto-nível, ou seja como diferentes componentes ao nível de ferramentas interagem entre si.
- **Arquitetura do Sistema (Baixo Nível).** Diagrama do sistema em uma visão de mais baixo-nível, descrevendo como os componentes em si podem ser implementados.
- **Conclusão.** Palavras finais em relação as expectativas do projeto e os impactos que o sucesso da aplicação pode gerar.

!!! nota

    Os títulos das seções não precisam, necessariamente, serem os descritos aqui.

## Documentação de Uso

Uma documentação de uso pode ser simples ou complexa.

- **Simples.** Documentada em um arquivo Markdown junto do repositório do projeto (`README.md`)
- **Complexa.** Documentação com seções dedicadas ao usuário e ao desenvolvedor (onde são documentados os componente do sistema em diferentes níveis de complexidade). Geralmente, definida em arquivos HTML.

### Simples

As seções de uma documentação de uso simples são:

- **Sobre**. Apresentação da aplicação (e, opcionalmente, o problema que visa resolver).
- **Pré-requisitos (Requisitos).** Aplicações ou pacotes que devem estar já presentes para o uso e instalação da aplicação.
- **Instalação.** Guia de instalação da aplicação
- **Quickstart.** Um exemplo rápido e simples de uso da ferramenta
- **Especificações.** Explicações sobre as funcionalidades do sistema, incluindo a intereção entre as diferentes funcionalidades e o workflow geral.
- **Exemplos.** Exemplos de uso da ferramenta em diferentes cenários.

### **Complexa**

As seções de uma documentação de uso complexa são:

- **Getting started.**
    - Installation (and pre-requisistes)
    - Quickstart
    - Why?
    - FAQ
    - Changelog
- **User Guide.**
- **API Reference.**
- **Developer Guide.**
    - Contributing
    - Bug report
    - Feature requests
    - Architecture Overview

Além disso, deve haver um `README.md` no repositório do projeto, onde são apresentadas referências para as respectivas seções na documentação.

Ainda, pode-se usar o Sphinx para a documentação automática da API, demais docstrings e arquivos `.rst` (ou `.md`). O Sphinx contém diversos temas interessantes que podem ser utilizados. Alguns exemplos são:

- [https://sphinx-themes.org/sample-sites/furo/](https://sphinx-themes.org/sample-sites/furo/)
- [https://sphinx-themes.org/sample-sites/sphinx-book-theme/](https://sphinx-themes.org/sample-sites/sphinx-book-theme/)