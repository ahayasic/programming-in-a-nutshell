# Classes e Objetos em Python

## Criando Classes

Para criar uma classe em Python, basta usar a _keyword_ `class` seguida do nome da classe

```python
class Point:
    pass
```

!!! note "Nota"
    De acordo com a PEP8, classes devem ser nomeadas em _CamelCase_

### Adicionando Atributos

Orientação à objetos é sobre entidades *(objetos)* que possuem dados *(atributos)* e comportamentos *(métodos)* que, geralmente, ocorrem através do uso dos dados.

No Python, um atributo é qualquer variável criada dentro da classe com prefixo `self`.

Um atributo pode receber qualquer objeto (literalmenmte, qualquer objeto).

Uma vez criado, o atributo pode ser acessado por meio da notação de ponto, da forma `<object>.<attribute>`.

```python
class Point:
    # Informações sobre o método __init__ adiante
    def __init__(self):
        self.x = 0
        self.y = 0
```

```python
p = Point()
p.x, p.y
```

    (0, 0)

Alternativamente, também podemos usar a notação de ponto (i.e. `<object>.<attribute> = <value>`) para criar um novo atributo em um objeto instanciado.

```python
p.z = 0

p.x, p.y, p.z
```

    (0, 0, 0)

### Adicionando Métodos

A única diferença entre métodos de uma classe e funções convencionais é que **todos os métodos possuem um argumento obrigatório**. Este argumento pode ser qualquer um, mas a convenção global é `self`.

!!! attention "Use a convenção!"

O argumento `self` é uma **referência para o objeto pelo qual o método está sendo invocado**. Assim, podemos acessar qualquer atributo ou método do objeto através do `self`.

!!! note "Nota"
    Ao invocarmos um método, o Python passa o argumento `self` automaticamente.

```python
class Point:
    # Informações sobre o método __init__ adiante
    def __init__(self):
        self.x = 0
        self.y = 0

    def reset(self):
        self.x = 0
        self.y = 0
```

```python
p = Point()
p.x = p.y = 3

p.x, p.y
```

    (3, 3)

```python
p.reset()
p.x, p.y
```

    (0, 0)

Perceba também que como temos a referência para o objeto, podemos fazer a chamada dos métodos a partir da classe (e não somente a partir objeto).

```python
p = Point()
Point.reset(p)

p.x, p.y
```

    (0, 0)

Se não declararmos o argumento obrigatório no nosso método, recebemos uma mensagem de erro indicando a falta de um argumento ao criarmos um objeto

```python
class Point:
    # Informações sobre o método __init__ adiante
    def __init__():
        self.x = 0
        self.y = 0
```

```python
p = Point()
```

    ----------------------------------------------

    TypeError    Traceback (most recent call last)

    <ipython-input-11-45eef628f0cf> in <module>
    ----> 1 p = Point()


    TypeError: __init__() takes 0 positional arguments but 1 was given

### Inicializando Objetos

O modo mais comum de se definir os atributos de um objeto é através de um construtor. Um construtor é um método especial executado sempre que um objeto é criado.

Em Python, há (na verdade) um construtor e um inicializador. Porém, o construtor é raramente utilizado. Assim, utilizamos o inicializador para definirmos todos os atributos da classe.

Assim como o construtor, o inicializador é sempre executado quando um objeto é criado e funciona da mesma forma que qualquer outro método. A única diferença está em seu nome, que deve ser `__init__`. Esta nomeação faz com que o interpretador do Python reconheça que `__init__` é o inicializador da classe.

```python
class Point:
    def __init__(self, x=0, y=0):
        self.move(x, y)

    def move(self, x, y):
        self.x = x
        self.y = y

    def reset(self):
        self.move(0, 0)
```

```python
p = Point(0, 0)
p.x, p.y
```

    (0, 0)

```python
p = Point(3, 5)
p.x, p.y
```

    (3, 5)


## Interface Privada

A grande maioria das linguagens orientadas à objetos possuem algum mecanismo de _controle de acesso_ aos métodos e atributos. Com isso, é possível marcar (e.g.) atributos como sendo:

- **Privados**. Apenas o próprio objeto pode acessá-lo.
- **Protegidos**. Apenas a classe e subclasses do objeto podem acessar seus atributos e métodos.

O controle de acesso é interessante principalmente para:

- **Encapsulamento de atributos.** Assim, quando o acesso ou atribuição de um atributo é complexo, podemos abstrair tal complexidade em métodos de _setting_ e _getting_ que devem ser utilizados para acessar ou definir o valor de um atributo.

- Quanto aos métodos, torná-los privados significa removê-los da interface principal cujo usuário deve utilizar para interagir com o objeto.

```python
class Person:

    def __init__(self, weight, height):
        self.set_weight(weight)
        self.set_height(height)

    def set_weight(self, weight):
        if weight < 0:
            raise ValueError("Weight cannot be lower than 0")
        self.weight = weight

    def set_height(self, height):
        if height < 0:
            raise ValueError("Height cannot be lower than 0")
        self.height = height
```

```python
p = Person(-1, -1)
```

    ----------------------------------------------

    ValueError   Traceback (most recent call last)

    <ipython-input-16-7429ca29f078> in <module>
    ----> 1 p = Person(-1, -1)


    <ipython-input-15-47f694df236e> in __init__(self, weight, height)
          2
          3     def __init__(self, weight, height):
    ----> 4         self.set_weight(weight)
          5         self.set_height(height)
          6


    <ipython-input-15-47f694df236e> in set_weight(self, weight)
          7     def set_weight(self, weight):
          8         if weight < 0:
    ----> 9             raise ValueError("Weight cannot be lower than 0")
         10         self.weight = weight
         11


    ValueError: Weight cannot be lower than 0

```python
p = Person(190, 90)
p.weight, p.height
```

    (190, 90)

O Python não posui um mecanismo de controle de acesso padrão. Tecnicamente, todos os métodos e atributos de uma classe são públicos. Porém, há algumas convenções que podemos utilizar para marcar-los como **internos** (ou seja, como algo encapsulado no objeto e que não deve ser acessado diretamente).

### Underscores

A forma mais comum de marcarmos atributos e métodos como internos é através de _underscores_ (`_`). Com isso, estamos deixando claro que o atributo em questão é interno e, portanto, não deve ser acessado diretamente.

- Note que isto é apenas uma convenção e que o usuário ainda pode acessá-lo, caso queira.

Alternativamente, podemos prefixar o atributo com dois _underscores_ (`__`). Assim, estamos:

- Recomendando fortemente para que o atributo não seja acessado diretamente
- Dificultando o acesso ao atributo, uma vez que o interpreteador do Python executará um processo denominado _"name mangling"_[^1] em todo atributo que comece com dois _underscore_

```python
class SecretString:
    '''A not-at-all secure way to store a secret string.'''
    def __init__(self, plain_string, pass_phrase):
        self.__plain_string = plain_string
        self.__pass_phrase = pass_phrase

    def decrypt(self, pass_phrase):
        '''Only show the string if the pass_phrase is correct.'''
        if pass_phrase == self.__pass_phrase:
            return self.__plain_string
        else:
            return ''
```

```python
secret_string = SecretString("ACME: Top Secret", "antwerp")

secret_string.decrypt("antwerp")
```

    'ACME: Top Secret'

```python
# Note que não é possível acessar o atributo diretamente através da notação de ponto
secret_string.__plain_text
```

    ----------------------------------------------

    AttributeErrorTraceback (most recent call last)

    <ipython-input-20-7d444361b25e> in <module>
          1 # Note que não é possível acessar o atributo diretamente através da notação de ponto
    ----> 2 secret_string.__plain_text


    AttributeError: 'SecretString' object has no attribute '__plain_text'

```python
# Porém, ainda podemos acessar o atributo através do nome resultante do processo de "name mangling"
secret_string._SecretString__plain_string
```

    'ACME: Top Secret'

### Properties

Uma segunda e mais robusta forma de definir atributos é através da função `property` (esta estratégia funciona apenas para atributos!).

Com `property` podemos

- Definir atributos que podem ser acessados diretamente (através da notação de ponto)
- (Ao mesmo tempo em que) estão encapsulados em alguma interface (i.e. em métodos de _setting_ e _getting_).

Podemos pensar na função `property` como uma função que:

- Retorna um objeto cujo objetivo é **"proxear"** qualquer requisição de atribuição ou acesso ao atributo para o método correspondente (que especificarmos).

A sintaxe da `property` é:

```python
property(fget=None, fset=None, fdel=None, doc=None)
```

onde,

- `fget` é a função que acessa o atributo
- `fset` é a função que atribui um valor ao atributo
- `fdel` é a função que deleta o atributo
- `doc` é uma string que representa a documentação do atributo.

Todos estes parâmetros são opcionais. Contudo, utilizarmos _properties_ com o objetivo de pelo menos definir uma função de acesso.

!!! warning "Atenção!"
    Mesmo com o uso de `property`, ainda é possível acessar e modificar os atributos via _name mangling_

```python
class Color:

    def __init__(self, rgb_value, name):
        self.rgb_value = rgb_value
        self.__name = name

    def _set_name(self, name):
        if not name:
            raise Exception("Invalid Name")
        self.__name = name

    def _get_name(self):
        return self.__name

    name = property(_get_name, _set_name)
```

```python
c = Color("#0000ff", "bright red")
c.name
```

    'bright red'

#### Decorador @property

Podemos usar a funçao `property` como um decorador para os métodos de _setting_ e _getting_, tornando a sintaxe mais simples e menos poluída.

Para isso, basta decorarmos o método _getter_ com `@property`, seguido do método cujo nome deve ser o mesmo do atributo. No caso de _setter_, basta utilizar a sintaxe `@<attribute>.setter / def <attribute>`

```python
class Color:

    def __init__(self, rgb_value, name):
        self.rgb_value = rgb_value
        self.__name = name

    @property
    def name(self):
        return self.__name

    @name.setter
    def name(self, name):
        if not name:
            raise Exception("Invalid Name")
        self.__name = name
```

```python
c = Color("#0000ff", "bright red")
c.name
```

    'bright red'

## Atributos e Métodos de Classe

Sabendo que objetos são instâncias que uma classe, pode haver situações em que os elementos da classe não são dependentes (i.e. estão relacionados) à instância da classe, mas apenas à classe em si.

Neste caso onde os elementos são os mesmos para qualquer instância da classe, o atributo e/ou método são denominados _da classe_.

### Atributos da Classe

No Python, qualquer variável declarada fora do inicializador ou métodos é uma variável da classe. Logo, um atributo da classe.

```python
class ExampleClass:
    attr_class = "I'm a class attribute"

    def __init__(self, attr_instance):
        self.attr_instance = f"I'm a instance attribute, look: '{attr_instance}'"
```

```python
a = ExampleClass("Salve!")
b = ExampleClass("Aloha!")
```

```python
a.attr_instance
```

    "I'm a instance attribute, look: 'Salve!'"

```python
b.attr_instance
```

    "I'm a instance attribute, look: 'Aloha!'"

```python
a.attr_class
```

    "I'm a class attribute"

```python
b.attr_class
```

    "I'm a class attribute"

```python
ExampleClass.attr_class
```

    "I'm a class attribute"

```python
ExampleClass.attr_instance
```

    ----------------------------------------------

    AttributeErrorTraceback (most recent call last)

    <ipython-input-31-ca4f4c1df6d3> in <module>
    ----> 1 ExampleClass.attr_instance


    AttributeError: type object 'ExampleClass' has no attribute 'attr_instance'

### Métodos da Classe

Assim como atributos de classe, métodos de classe possuem o mesmo princípio.

A questão é que no Python, podemos diferenciar métodos de classe em dois tipos: `staticmethod` e `classmethod`.

- `staticmethod`. Métodos onde não há qualquer passagem implícita de parâmetros. De fato, são métodos idênticos a funções convencionais onde a única diferença é que para executá-lo é necessário invocá-lo através da classe ou objeto (instância da classe).

- `classmethod`. Métodos onde ao invés da referência para o objeto (`self`) ser passada implícitamente como parâmetro, é passado a referência para a classe do objeto. Assim como o `staticmethod`, para executá-lo basta invocá-lo através da classe ou objeto.

```python
class ExampleClass:

    def foo(self):
        print(f"I'm foo from {self}")

    @classmethod
    def class_foo(cls):
        print(f"I'm foo from {cls}")

    @staticmethod
    def static_foo():
        print(f"I'm a static foo")
```

```python
a = ExampleClass()
```

```python
a.foo()
```

    I'm foo from <__main__.ExampleClass object at 0x7f2c0806e280>

```python
a.class_foo()
```

    I'm foo from <class '__main__.ExampleClass'>

```python
a.static_foo()
```

    I'm a static foo

Diferente de outras linguagens de orientação à objetos, em Python o uso de métodos de classe é questionável.

Isso porque, o uso recomendado de `classmethod` é para a sobrecarga de "construtores" (na verdade, inicializadores) de subclasses, uma vez que o uso de `@classmethod` é a melhor alternativa para acessar a classe do objeto.

Caso acessar a classe não seja o objetivo, então podemos usar `staticmethod`. Contudo, devemos reconsiderar fortemente se o método em questão deve realmente estar vinculado à classe ou, então, pode ser definido uma função independente (_standalone_).

## Boas Práticas na Criação de Classes

### Quando Usar Interface Privada

In general, always use a standard attribute until you need to control access to that property in some way. In either case, your attribute is usually a noun. The only difference between an attribute and a property is that we can invoke custom actions automatically when a property is retrieved, set, or deleted.

### Underscore vs Properties

Use a convenção (de _underscores_) caso:

- A classe não exija muita abstração
- A quantidade de atributos é pequena

Caso contrário use `@property`, ou seja:

- A classe exige muita abstração
- A quantidade de atributos é grande
- A vários atributos derivados

Ainda, embora mais complexo, opte sempre pelo uso de dois **underscores**.

### Setters e Getters

Visto que podemos criar atributos em qualquer método da classe, ao inicializarmos atributos privados, os métodos de _setting_ e _getting_ devem seguir o padrão de nomenclatura (quando usando _underscores_):

- `set_<attribute>`
- `get_<attribute>`

## Examples

### Example \#1

```python
class Person:

    def __init__(self, age, weight, height):
        self.set_age(age)
        self.set_weight(weight)
        self.set_height(height)
        self.set_bmi(self.get_weight(), self.get_height())

    def get_age(self):
        return self.__age

    def set_age(self, age):
        if age < 0:
            raise ValueError("Age cannot be lower than 0")
        self.__age = age

    def get_weight(self):
        return self.__weight

    def set_weight(self, weight):
        if weight < 0:
            raise ValueError("Weight cannot be lower than 0")
        self.__weight = weight

    def get_height(self):
        return self.__height

    def set_height(self, height):
        if height < 0:
            raise ValueError("Height cannot be lower than 0")
        self.__height = height

    def get_bmi(self):
        return self.__bmi

    def set_bmi(self, weight, height):
        self.__bmi = weight / ( (height / 100) ** 2)
```

```python
p = Person(21, 90, 190)
p.get_age(), p.get_weight() , p.get_height(), p.get_bmi()
```

    (21, 90, 190, 24.930747922437675)

### Example \#2

```python
class Person:

    def __init__(self, age, weight, height):
        self.age = age
        self.weight = weight
        self.height = height
        self.bmi = None

    @property
    def age(self):
        return self.__age

    @age.setter
    def age(self, age):
        if age < 0:
            raise ValueError("Age cannot be lower than 0")
        self.__age = age

    @property
    def weight(self):
        return self.__weight

    @weight.setter
    def weight(self, weight):
        if weight < 0:
            raise ValueError("Weight cannot be lower than 0")
        self.__weight = weight

    @property
    def height(self):
        return self.__height

    @height.setter
    def height(self, height):
        if height < 0:
            raise ValueError("Height cannot be lower than 0")
        self.__height = height

    @property
    def bmi(self):
        return self.__bmi

    @bmi.setter
    def bmi(self, null):
        self.__bmi = self.weight / ( (self.height / 100) ** 2)
```

```python
p = Person(21, 90, 190)
p.bmi
```

    24.930747922437675

```python
p._Person__bmi
```

    24.930747922437675

[^1]: O processo de _"name mangling"_ é um processo onde qualquer identificador com dois _underscore_ de prefixo ou um _underscore_ de sufixo é textualmente substituído por `_className__identifier`, onde `_className` é o nome da classe corrente (ao qual o identificador pertence).
