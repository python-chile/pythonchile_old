---
layout:     post
title:      Pruebas unitarias en el Python
author:     murilobsd
github:		murilobsd
tags: 		pruebas python2 python3
subtitle:   pruebas unitarias en el python en la práctica  
category:   
---
<!-- Start Writing Below in Markdown -->

```
Write Test --> Test Fails --> Write Code --> Test Passes --> Refactor <--> Write Teste ....
```

Python tiene una biblioteca incorporada para pruebas unitarias
[unittest](https://docs.python.org/3/library/unittest.html). El objetivo de
este post será crear una función para comprobar si hay dígito **0,1,2,3,4,5,6,7,9**
en una determinada frase, en caso afirmativo retorna verdadero si no falso.

Iniciaremos la creación de un archivo con el prefijo de **test**

{% highlight bash %} 
$touch test_comprobar_digito.py 
{% endhighlight %}


En el archivo vamos a crear una clase que hereda de 
[TestCase](https://docs.python.org/3/library/unittest.html#unittest.TestCase),
en la que tendremos varios métodos listos para facilitar en los casos, vea que
los métodos necesitan tener el prefijo de **test**.

{% highlight python %}
# coding:utf-8

import unittest

class TestCheckDigit(unittest.TestCase):

    def test_comprobar_digito_ok(self):
        pass
{% endhighlight %}

En el método anterior utilizamos el estándar
[Arrange-Act-Assert](http://c2.com/cgi/wiki?ArrangeActAssert), en el cual
primero definimos los **inputs** después **actuamos** en el método y por fin hacemos la
**aserción**.

{% highlight python %}
# coding:utf-8

import unittest

class TestCheckDigit(unittest.TestCase):

    def test_comprobar_digito_ok(self):
        # input
        sentencia = "hoy es el 2 de julio de 2018, Brasil ganó 2x0 de México en la copa del mundo."

        # act
        hay_digito = comprobar_digito(sentencia)

        # assert
        self.assertTrue(existe_digito)
{% endhighlight %}

La clase [TestCase](https://docs.python.org/3/library/unittest.html#unittest.TestCase) proporciona [varios métodos para aserciones](https://docs.python.org/3/library/unittest.html#unittest.TestCase.debug)


Para ejecutar la prueba anterior, basta con escribir el comando abajo:

{% highlight bash %} 
$python -m unittest test_comprobar_digito
{% endhighlight %}

Tendremos una salida parecida a la siguiente:

```
E
======================================================================
ERROR: test_comprobar_digito_ok (test_comprobar_digito.TestCheckDigit)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "test_comprobar_digito.py", line 12, in test_comprobar_digito_ok
    hay_digito = comprobar_digito(sentencia)
NameError: global name 'comprobar_digito' is not defined

----------------------------------------------------------------------
Ran 1 test in 0.000s

FAILED (errors=1)
```

Op's pero no importamos ni creamos la función de **buscar_digito**, por eso
que ha devuelto un error. Vamos a arreglar el error.

Crear un archivo:

{% highlight bash %} 
$touch comprobar.py 
{% endhighlight %}

Dentro de ese archivo creamos la función:

{% highlight python %}
# coding:utf-8

def comprobar_digito():
    pass

{% endhighlight %}

Ahora importaremos la función dentro de nuestra prueba.

{% highlight python %}
# coding:utf-8

import unittest

from comprobar import comprobar_digito

class TestCheckDigit(unittest.TestCase):

    def test_comprobar_digito_ok(self):
        # input
        sentencia = "hoy es el 2 de julio de 2018, Brasil ganó 2x0 de México en la copa del mundo."

        # act
        hay_digito = comprobar_digito(sentencia)

        # assert
        self.assertTrue(existe_digito)
{% endhighlight %}

De nuevo, ejecute el comando para ejecutar las pruebas:

{% highlight bash %} 
$python -m unittest test_comprobar_digito
{% endhighlight %}

Op's más un error, deje que la prueba nos guíe !!!

```
E
======================================================================
ERROR: test_comprobar_digito_ok (test_comprobar_digito.TestCheckDigit)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "test_comprobar_digito.py", line 14, in test_comprobar_digito_ok
    hay_digito = comprobar_digito(sentencia)
TypeError: comprobar_digito() takes no arguments (1 given)

----------------------------------------------------------------------
Ran 1 test in 0.000s

FAILED (errors=1)

```

Necesitamos recibir un argumento en nuestra función:

{% highlight python %}
# coding:utf-8

def comprobar_digito(sentencia):
    pass

{% endhighlight %}

```
F
======================================================================
FAIL: test_comprobar_digito_ok (test_comprobar_digito.TestCheckDigit)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "test_comprobar_digito.py", line 17, in test_comprobar_digito_ok
    self.assertTrue(hay_digito)
AssertionError: None is not true

----------------------------------------------------------------------
Ran 1 test in 0.000s

FAILED (failures=1)

``` 

Y la salida mejoró, ahora está volviendo una falta, entonces concertamos
todos los errores de semántica del código ahora realment la aserción
está fallando.

{% highlight python %}
# coding:utf-8

def comprobar_digito(sentencia):
    return True

{% endhighlight %}

``` 
.
----------------------------------------------------------------------
Ran 1 test in 0.000s

OK
``` 

Perfecto ahora la prueba ha pasado ;)

¡Incluso la prueba pasando quedó aquella duda! Pero es que mi función
está teniendo el comportamiento correcto !? Bueno para eso haremos un nuevo.
prueba.

{% highlight python %}
# coding:utf-8

import unittest

from comprobar import comprobar_digito

class TestCheckDigit(unittest.TestCase):

    def test_comprobar_digito_ok(self):
        # input
        sentencia = "hoy es el 2 de julio de 2018, Brasil ganó 2x0 de México en la copa del mundo."

        # act
        hay_digito = comprobar_digito(sentencia)

        # assert
        self.assertTrue(hay_digito)
    
    def test_comprobar_digito_error(self):
        # input
        sentencia = "Chi chi chi le le le."

        # act
        hay_digito = comprobar_digito(sentencia)

        # assert
        self.assertFalse(hay_digito)
{% endhighlight %}

De nuevo ejecutamos las pruebas para ver si todo está bien.

```
F.
======================================================================
FAIL: test_comprobar_digito_error (test_comprobar_digito.TestCheckDigit)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "test_comprobar_digito.py", line 27, in test_comprobar_digito_error
    self.assertFalse(hay_digito)
AssertionError: True is not false

----------------------------------------------------------------------
Ran 2 tests in 0.000s

FAILED (failures=1)
```

¡Que bueno! Nuestra función realmente no está teniendo el comportamiento previsto,
vamos a refactorizarla.

{% highlight python %}
# coding:utf-8

def comprobar_digito(sentencia):
    for carta in sentencia:
        if carta.isdigit():
            return True
    return False
{% endhighlight %}

Realizamos nuevamente las pruebas:

```
..
----------------------------------------------------------------------
Ran 2 tests in 0.000s

OK
```

Perfecto, tenemos una función que cheque si hay dígito o no en una determinada
frase, cubierta de pruebas y creada con TDD.