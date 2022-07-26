# Linting

## Introdução

Durante os momentos de escrita de código podemos cometer uma série de erros:

- Erros de sintaxe.
- Erros lógicos que prejudicam a segurança da aplicação.
- Erros de estilização do código.

Porém, tais erros são normais de acontecerem e, mesmo com muito esforço, é difícil encontrarmos todos eles.

Portanto, utilizamos ferramentas capazes de analisar e detectar problemas em nossos códigos. Essas ferramentas, chamadas de linters, são capazes de checar:

- Erros lógicos (Logical Lint)
    - Erros de sintaxe.
    - Pedaços de código com resultados indesejados (bugs).
    - Pedaços de código com falhas de segurança.
- Erros de estilização (Stylistic Lint)
    - Código não alinhado com o Guia de Estilo definido.

Há também outras ferramentas que podemos usar em complemento aos linters que nos ajudam a melhorar a qualidade do código, como é o caso de formatadores automáticos de código (code formatters).

!!! info "Nem tudo é o que parece"

    Na maioria dos casos, os linters que usamos são, na verdade, uma combinação de vários linters que detectam problemas específicos.

## Linting: flake8

A linguagem de programção Python possui vários linters. Um famoso, é o flake8, capaz de detectar erros lógicos e de estilização (PEP8), sendo uma combinação dos linters:

- PyFlakes
- pycodestyle (PEP8)
- Mccabe

A instalação do flake8 é simples, basta executarmos:

```shell
pip install flake8
```

Se o seu ambiente de desenvolvimento possuir suporte para linting automático (e.g. Visual Studio Code), os erros serão sinalizados no próprio ambiente.

Caso contrário, podemos executar o linting especificamente nos arquivos desejados

```shell
flake8 path/to/files/
```

Ainda, podem haver erros ou avisos que gostaríamos de omitir por ser um falso-positivo ou que ao invés de contribuir para a melhoria da qualidade do código, age ao contrário.

Nesse caso, basta adicionamos a flag `--ignore=` seguido dos códigos de erros que desejamos ignorar.

```shell
flake8 --ignore=E1,E23,W503 path/to/files
```

Se o objetivo for ignorar apenas linhas específicas, basta incluirmos a flag `# noqa:` seguida dos códigos de erro.

```python
example = lambda: 'example'  # noqa: E731,E123
```

!!! tip "Dica"

    Para ignorar todo e qualquer tipo de erro (ou aviso), use apenas `# noqa`

### Arquivos de Configuração

A fim de garantir que todas as pessoas que estiverem trabalhando no mesmo projeto sigam as mesmas diretrizes, podemos definir as configurações do flake8 em arquivos de configuração na raíz do projeto.

As configurações podem estar tanto em um arquivo `.flake8` ou em `setup.cfg` ou `tox.ini`.

Basta então adicionar a chave `[flake8]`seguida das configurações desejadas. Por exemplo:

```ini
[flake8]
ignore = D203
exclude =
    .git,
    __pycache__,
    docs/source/conf.py,
    old,
    build,
    dist
max-complexity = 10
```

## Going Deeper

No caso de linguagens tipadas dinamicamente, como Python, mas que suportam *typing annotation*, podemos usar ferramentas adicionais para garantir que os tipos, quando anotados, se comportam como o esperado.

É o caso da ferramenta mypy.

### Typing hinting com mypy

Python é uma linguagem dinamicamente tipada, logo as tipagens ocorrem em tempo de execução (ou seja, conforme o código é executado).

Porém, há situações em que queremos garantir que os tipos da variáveis sejam respeitados, como, por exemplo, na passagem de parâmetros para uma função ou no retorno desta.

A ferramenta mypy é uma *type checker*  capaz de analisar e validar se as anotações de tipo são respeitadas com base nas [PEP 484](https://www.python.org/dev/peps/pep-0484) e [526](https://www.python.org/dev/peps/pep-0526).

Para instalar o `mypy`, basta rodar

```shell
pip install mypy
```

Então, para fazer a validação das anotações de tipo, executamos:

```shell
mypy script.py
```

!!! note "Nota"

    O mypy iŕa executar toda a análise de forma estática. Isto é, assim como um linter, não será executada qualquer linha de código.

### Typing hinting com Pyright

Pyright é um type checker (assim como mypy) com foco em analisar grandes bases de código Python, especialmente em modo "watch". As análises são feitas de forma incremental conforme os arquivos são modificados, possibilitando que grandes conjuntos de arquivos sejam verificados rapidamente, enquanto escritos.

Ainda, o Pyright é uma ferramenta integrada ao Pylance que, por sua vez, faz parte do ecossistema de extensões Python da Microsoft para o Visual Studio Code.

Para ativá-lo no VSCode, basta abrir o painel de configurações e procurar por `type checking`. O resultado deve ser:

```
Python › Analysis: Type Checking Mode
Defines the default rule set for type checking.
```

As opções de regras são:

- `basic`. Checagem simples, sem muitas restrições.
- `strict`. Checagem completa, com diversas restrições e alta sensibilidade a possíveis erros.

Também é possível configurar o pyright via arquivo de configurações (e.g. `settings.json`). Neste caso, basta adicionar ao arquivo:

```json
{
  "python.analysis.typeCheckingMode": "basic"  # or `strict`
}
```

!!! warning "Typing hints podem ser legais?"
    Para um material prático sobre o uso de typing hints em Python, veja a seção [Typing Hints](../mastering_python/typing_hints.md)

### Linting durante testes

A fim de garantir que toda adição ou alteração de código no projeto respeite as convenções de estilo e padrões de qualidade, podemos incluir a validação de linters como uma etapa de teste (que é executada durante um pipeline de CI/CD) ou mesmo antes de um commit ser feito (considerando que há o uso de Git).

Para tal, podemos usar a ferramenta **pre-commit**, GitHub Actions ou GitLab CI/CD.