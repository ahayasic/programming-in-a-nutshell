# Guia de Importações em Python

!!! warning "Attention! Atenção!"
    **(en_US)** This document is a translation and adaptation of the publication ["The Definitive Guide to Python import Statements"](https://chrisyeh96.github.io/2017/08/08/definitive-guide-python-imports.html#example-directory-structure) by [Chris Yeh](https://chrisyeh96.github.io/).  I just made a few tweaks for my own use. For complete instructions, visit the original publication!

    **(pt_BR)** Este documento é uma traduação e adaptação da publicação ["The Definitive Guide to Python import Statements"](https://chrisyeh96.github.io/2017/08/08/definitive-guide-python-imports.html#example-directory-structure) de [Chris Yeh](https://chrisyeh96.github.io/). Eu apenas fiz alguns ajustes para uso próprio. Para instruções completas, visite a publicação original!

## Terminologia

- **Módulo.** Todo e qualquer arquivo `.py`
- **Módulos Built-in**: Módulos padrões da linguagem Python, geralmente escritos em C e traduzidos pra Python.
- **Pacote.** Até o Python 3.3, qualquer diretório que contenha o arquivo `__init__.py`. A partir do Python 3.3, qualquer diretório que contenha ou não `__init__.py`
- **Objeto.** Tudo do Python

## Estrutura de Diretórios (de Exemplo)

```
test/                      # root folder
    packA/                 # package packA
        subA/              # subpackage subA
            __init__.py
            sa1.py
            sa2.py
        __init__.py
        a1.py
        a2.py
    packB/                 # package packB (implicit namespace package)
        b1.py
        b2.py
    math.py
    random.py
    other.py
    start.py
```

## Sobre o `import`

Quando executamos um `.py` (e.g. `python foo.py`), o interpretador do Python busca todos os módulos ou pacotes na seguinte ordem:

- Módulos Built-in
- Módulos e pacotes importáveis através dos caminhos definidos em `sys.path`.

A variável `sys.path` é uma lista de caminhos a partir do qual podemos importar pacotes ao executarmos o `.py`. Assim, a `sys.path` sempre é inicializada com os seguintes caminhos:

- Diretório corrente onde o `.py` está sendo executado.
- Diretórios contidos na variável de ambiente `PYTHONPATH`.
- Diretórios padrões do `sys.path`. Ou seja: Python Standard Library, pacotes instalados no ambiente virtual, etc.

Logo, **apenas** os pacotes **contidos nos caminhos** de `sys.path` são **importáveis**.

Além disso,

- Se o (interpretador) Python for invocado iterativamente, `sys.path[0]` é uma _string_ vazia, indicando que os módulos e pacotes devem ser pesquisados através do diretório corrente (a partir do interpretador foi chamado).
- Já se o script for executado da forma `python foo.py`, então `sys.path[0]` é o caminho absoluto para o script em questão.

Portanto, perceba que **quando executamos um script Python, pouco importa qual é o caminho do seu "diretório de trabalho", o que importa é o caminho do script**.

!!! example "Exemplo"
    Supondo que a localização do nosso *shell* estiver em `test/`, se executarmos `python ./packA/subA/sa1.py`.

    - `sys.path[0]` será `test/packA/subA/`, mas não `test/`.

Além disso, **`sys.path` é compartilhada entre todos os módulos importáveis!**

!!! example "Exemplo"
    Suponha que façamos `user@user:~/test$ python start.py`, onde

    ```python
    # test/start.py
    from packA import a1
    ```

    ```python
    # test/packA/a1.py
    import sys
    print(sys.path)
    ```

    Com isso, `sys.path[0]` será `test/` e, portanto, o módulo `a1.py` será capaz de importar todo e qualquer módulo e pacote que parte de `test/`.

## Sobre o `__init__.py`

O arquivo `__init__.py` tem dois papéis:

- Até o Python 3.3, `__init__.py` era responsável por tornar um pacote de scripts em um pacote de módulos importáveis.
- (Para toda e qualquer versão do Python) Responsável por executar o código de inicialização dos pacotes.

Logo, para que os pacotes sejam considerados como módulos importáveis e inicializados apropriadamente (até o Python 3.3), devemos incluir o `__init__.py` no pacote em questão, como apresentado na [estrutura de diretórios](#estrutura-de-diretorios-de-exemplo).

Já para versões do Python superiores à 3.3, todos os diretórios são considerados como pacotes, devido a adoção do pacotes de *namespaces* implícitos.

## Inicialização dos Pacotes

Sempre que um pacote (ou um dos seus módulos) for importado, o Python executa todo o código contido em `__init__.py` (presente na raíz do pacote), caso exista. Com isso, todos os objetos importáveis definidos em `__init__.py` são considerados parte do _namespace_  do pacote[^1].

[^1]: Isso significa que podemos importá-los através da sintaxe `pkgname.<obj_to_import>`

Por exemplo, sejam:

```python
# test/packA/a1.py
def a1_func():
	print("running a1_func()")
```

```python
# test/packA/__init__.py

# this import makes a1_func directly accessible from packA.a1_func
from packA.a1 import a1_func

from packA_func():
    print("running packA_func()")
```

```python
# test/start.py
import packA

packA.packA_func()
packA.a1_func()
packA.a1.a1_func()
```

A saída do comando `user@user:~/test$ python start.py` é:

```shell
running packA_func()
running a1_func()
running a1_func()
```

## Sintaxe para Importação de Objetos

Como já visto anteriormente, podemos executar importações de 3 formas diferentes

1. `import <package or module>`

    !!! example "Exemplo"
        ```python
        import numpy
        ```

2. `import <package or module> as alias`

    !!! example "Exemplo"
        ```python
        import numpy as np
        ```

3. `from <package or module> import <inner>`

    !!! example "Exemplo"
        ```python
        from numpy import random
        ```


No primeiro caso, após a importação, basta navegarmos entre os diferentes níveis do pacote através da notação de ponto. Por exemplo, `pkgX.subpkgX.moduleX.X`

No segundo caso, definimos um "apelido" para o pacote ou módulo importado. Assim, podemos utilizá-lo através do alias.

Já no terceiro caso, importamos um subpacote, módulo ou objeto mais interno do pacote principal.

Note ainda que a notação de ponto pode ser utilizada para importar diretamente os pacotes de interesse (em conjunto com `from`). Por exemplo `from pkgX.subpkgX.moduleX import X`.

Por fim, lembre que apenas os objetos declarados no `__init__.py` de cada pacote (ou subpacote) são importáveis.

### Importação Absoluta vs Relativa

**Importação absoluta** é aquela que usa o caminho absoluto (ou seja, a partir do diretório raiz do projeto) para o módulo que se deseja importar.

**Importação relativa** usa o caminho relativo (ou seja, a partir do módulo corrente) para o módulo que se deseja importar. Ainda, há dois tipos de *importação relativa*.

- **Importação relativa *explícita*.** Importações no formato `from .<module or package> import X`, onde `.` indica o diretório corrente; `..` indica o diretório anterior, etc.
- **Importação relativa *implícita*.**  Importações definidas como se o diretório atual fosse parte de `sys.path`. Contudo, note que **IMPORTAÇÕES RELATIVAS IMPLÍCITAS NÃO EXISTEM NO PYTHON 3!!!**

Note ainda que, na importação relativa conseguimos importar módulos apenas até o nível a partir do qual estamos executando o `.py`.

Portanto, é recomendado o uso de importação absoluta na grande maioria dos casos. Afinal, a importação absoluta evita confusões e inconsistências. Além disso, scripts que usam importação relativa não podem ser executados diretamente.

## Troubleshooting

Uma das grandes vantagens da linguagem Python é sua flexibilidade e durante o desenvolvimento de scripts também buscamos flexibilidade em como os módulos se relacionam entre si.

Logo, seja para rodar um script diretamente ou importar um módulo dentro de outro, a flexibilidade é importante. Porém, o comportamento de importação do Python produz algumas complicações especiais:

- **Ao executarmos um script diretamente, é impossível importar qualquer coisa de seu diretório-pai.**
- **O `sys.path` é diferente para cada script, quando executados diretamente a partir da pasta em que cada um se encontra.**

### Exemplo

Vamos considerar a seguinte situação. Dada nossa [esturutra de diretórios](#estrutura-de-diretorios-de-exemplo), queremos:

- Executar tanto o script `start.py` quanto `a2.py` diretamente.
- O `start.py` importa o módulo `a2.py`.
- O `a2.py` importa o módulo `sa2.py`.

#### Problema
Ao executarmos `user@user:~test/$ python start.py`, o `sys.path` terá `test/` e as declarações de importações serão:

```python
# test/start.py
from packA import a2
```

```python
# test/packA/a2.py
from packA.subA import sa2
```

Porém, ao executarmos ` user@user:~test/packA$ python a2.py`, o `sys.path` terá `test/packA/`. Logo:

- A declaração de importação `from packA.subA import sa2` não irá funcionar, pois `packA` não é um diretório contido em `test/packA/`.
- Se trocarmos a declaração para `from subA import sa2`, será possível executar `python a2.py`, mas impossível executar `python start.py`, pois não há `subA/` em `sys.path`.

#### Soluções (Workarounds)
Consideando estes problemas, podemos recorrer às seguintes estratégias:

1. Use a importação absoluta (sempre partindo da raíz, no caso `test/`) em todos os módulos, executando módulos mais internos a partir da raíz **(recomendado)**.

    - Com isso, você será capaz de executar o `start.py` diretamente.
    - Para executar o `a2.py` diretamente, execute-o como um módulo:

        - Vá para a raiz do diretório de trabalho
        - Execute `a2.py` como um módulo importado

        ```shell
        python -m packA.a2
        ```

2. Use a importação absoluta (sempre partindo da raíz, no caso `test/`) em todos os módulos, sendo o diretório raíz adicionado na `sys.path`.

    - Com isso, você será capaz de executar o `start.py` diretamente.
    - Para executar o `a2.py`, adicione o diretório raíz `test/` antes da importação de `sa2.py`

    ```python
    import os, sys
    sys.path.append(os.path.dirname(os.path.dirname(os.path.realpath(__file__))))

    from packA.subA import sa2
    ```

    !!! note "Nota"
        Este método funciona no geral. Contudo, há situações em que a variável `__file__` pode estar incorreta. Neste caso, precisamos de uma solução mais robusta. Acesse essa [resposta no StackOverflow](https://stackoverflow.com/a/11158224) para mais instruções.

3. Instale o pacote em modo de desenvolvimento no ambiente virtual. Com isso, a raíz do diretório sempre estará presente no `sys.path` como um pacote instalado **(recomendado)**.

    !!! tip "Dica"
        Para informações sobre criação e instalação de pacotes, veja [Criando Pacotes e Módulos](#).

## Referências

- [The Definitive Guide to Python import Statements](https://chrisyeh96.github.io/2017/08/08/definitive-guide-python-imports.html#example-directory-structure)

