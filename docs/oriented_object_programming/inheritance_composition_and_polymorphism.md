# Herança e Composição

## Terminologia

- **Classe base/Super classe.** Classe a partir do qual outras classes derivam.
- **Classe derivada/Subclasse.** Classe derivada (i.e. que herda) de uma classe base.

## Introdução

Herança e composição são dois conceitos fundamentais na orientação a objetos que modelam **relacionamentos** entre duas classes. 

## Herança

A herança é um conceito modela uma relação do tipo **"isto é"** (is a). Logo, _classes derivadas_ de uma _classe base_ são especializações desta classe base.

Herança é usada principalmente para **incrementar** ou **modificar** (i.e. especializar) as funcionalidades de uma classe base, enquanto as outras funcionalidades permanecem iguais. Com isso, podemos:
- Adicionar comportamentos
- Modificar comportamentos

Em Python, a sintaxe de uma herança é da forma `class <SubClassName>(<SuperClassName>):`


```python
class Animal:
    """Classe base: Animal"""
    def walk(self):
        print("I'm an animal and I walk")

class Cachorro(Animal):
    """Classe derivada: Cachorro"""
    def play(self):
        print("I'm an dog and I play")
```


```python
c = Cachorro()
```


```python
c.walk()
```

    I'm an animal and I walk



```python
# Diferente de instâncias da classe Animal
# objetos da classe cachoro possuem um comportamento adicional "play" (brincar)

c.play()
```

    I'm an dog and I play


### `super()`

Há diversas situações onde, a fim de evitar duplicação de código, queremos aproveitar de funcionalidades já implementadas na classe base. Por exemplo, considere a seguinte situação:


```python
class Animal:

    def __init__(self, weight):
        self.weight = weight


class Cachorro(Animal):

    def __init__(self, weight, sound):
        self.weight = weight
        self.sound = sound
```

Note que na classe `Cachorro`, não é interessante replicarmos a lógica de atribução do dado `self.weight`. A melhor abordagem seria aproveitar da lógica já existente na classe base.

Podemos fazer isso usando a função `super()`. A `super()` é uma função que retorna uma instância da classe base, permitindo-nos chamar diretamente seus métodos.

O uso mais comum de `super()` é para inicializar atributos definidos na classe base.


```python
class Animal:

    def __init__(self, weight):
        self.weight = weight


class Cachorro(Animal):

    def __init__(self, weight, sound):
        super().__init__(weight)
        self.sound = sound
```


```python
animal = Animal(10)
animal.weight
```




    10




```python
cachorro = Cachorro(10, "Au Au")
cachorro.weight, cachorro.sound
```




    (10, 'Au Au')




```python
class BaseClass:
    def __init__(self):
        pass

class DerivedClass(BaseClass):

    def __init__(self):
        inst = super()
        print(inst, "\n", type(inst))
```


```python
base = BaseClass()
```


```python
derived = DerivedClass()
```

    <super: <class 'DerivedClass'>, <DerivedClass object>> 
     <class 'super'>


### Sobrecarga de Métodos

As vezes, ao invés de adicionar funcionalidades, queremos modificá-las.

Por exemplo, podemos considerar que todo animal emite um som. Porém, o som de um gato é diferente de um cachorro. Logo, ambas entidades implementam um método `sound()`, mas cada método executa uma lógica diferente.


```python
class Animal:
    def sound(self):
        print("I'm an animal!")

class Cachorro(Animal):
    def sound(self):
        print("Au au")

class Gato(Animal):
    def sound(self):
        print("Miau")
```


```python
animal = Animal()
animal.sound()
```

    I'm an animal!



```python
cachorro = Cachorro()
cachorro.sound()
```

    Au au



```python
gato = Gato()
gato.sound()
```

    Miau


#### Polimorfismo

A sobrecarga de métodos é uma das formas mais comuns de implementar o **polimorfismo**, assim como uma das principais motivações para o uso de herança em diversos contextos de orientação à objetos.

O polimorfismo é o mecanismo utilizado para definirmos lógicas diferentes para um mesmo comportamento em cada classe derivada. 

- Em outras palavras, podemos implementar lógicas diferentes para métodos com mesma assinatura (em classes derivadas).

O exemplo de animais, com gato e cachorro é um caso de polimorfismo, onde as subclasses `Cachorro` e `Gato` possuem sua própria implementação do método (comportamento) `sound()`.


#### Duck Typing
Embora importante, o polimorfismo em Python é um tanto "desinteressante" devido ao *duck typing*.

**Duck typing** é um estilo de programação (possível em linguagens dinamicamente tipadas) em que o tipo do objeto não importa. O que importa é apenas a presença ou não do comportamento desejado pelo objeto.

Em outras palavras, *não precisamos saber o tipo do objeto para invocarmos um método específico. Se ele contém este método, então invocamos.*

!!! info "Curiosidade"
    O termo *"Duck typing"* vem da expressão "Se parece um pato e fala como um pato, então provavelmente é um pato"

Por exemplo, considerando o exemplo dos animais:


```python
class Pato:
    def sound(self):
        print("Quack quack")
```


```python
def make_sound(animal):
    animal.sound()
```


```python
make_sound(cachorro)
```

    Au au



```python
pato = Pato()
make_sound(pato)
```

    Quack quack


Note que pouco importa o tipo do objeto passado como argumento. Seja `Animal` ou não, contanto que implemente o método desejado, podemos invocá-lo.

Dessa forma, o uso de herança continua sendo interessante para o compartilhamento de um código único entre diversas subclasses. Contudo, caso o código compartilhado seja apenas uma interface pública, podemos reconsiderar a implementação aproveitando do *duck typing*.

Contudo, é fato que mesmo que um objeto satisfaça uma interface em particular, isso não significa que ele irá funcionar em todas as situações. Portanto, a escolha entre herança, polimorfismo e *duck typing* precisa ser bem pensada.

### Classes Abstratas

Embora o *duck typing* seja útil, nem sempre é fácil ou manutenível criar classes que contém toda a funcionalidade necessária. Assim, podemos usar as classes abstratas base.

Classes abstratas base (ou ABCs) são classes que definem um conjunto mínimo (e fundamental) de métodos e atributos que uma classe deve implementar. Com isso, podemos criar classes derivadas que possuem sua própria implementação de cada funcionalidade da classe abstrata base.

!!! note "Nota"
    Perceba que a classe base não pode ser instanciada.

    Além disso, as classes derivadas **precisam** implementar todas as funcionalidades definidas na classe abstrata base.

## Composição

A composição modela uma relação do tipo **"composto de"** (has a). Isto significa que a classe em questão é composta (ou seja, contém dados/atributos) pela combinação de diferentes objetos. No geral, chamamos essa instância que compõe a classe de _componente_.


```python
class Motor:
    pass


class Carro:

    def __init__(self, cor, modelo, ano):
        self.__cor = cor
        self.__modelo = modelo
        self.__ano = ano
        self.__motor = Motor()
```

## Examples

!!! info "Work in progress"