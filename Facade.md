ПАТТЕРНЫ ПРОЕКТИРОВАНИЯ
=======================

* * *

Авторы:
-------

Шамсутдинова Оксана  
Шипаева Лариса

Паттерн Фасад (Facade)
=============================

* * *

**Фасад** — это структурный паттерн проектирования, который предоставляет простой интерфейс к сложной системе классов, библиотеке или фреймворку.

Проблема
--------

Представьте, что у нас есть стиральная машина, которая может стирать одежду, полоскать ее и отжимать, но все задачи выполняет отдельно. Поскольку вся система довольно сложна, нам нужно абстрагироваться от сложностей подсистем. Нам нужна система, которая может автоматизировать всю задачу без помех с нашей стороны.

Решение
--------

Для решения описанной выше проблемы мы хотели бы использовать фасадный метод. Это поможет нам скрыть или абстрагировать сложности подсистем следующим образом.

Следующий код написан с использованием фасадного метода
```python
class Washing:
	'''Subsystem # 1'''

	def wash(self):
		print("Washing...")


class Rinsing:
	'''Subsystem # 2'''

	def rinse(self):
		print("Rinsing...")


class Spinning:
	'''Subsystem # 3'''

	def spin(self):
		print("Spinning...")


class WashingMachine:
	'''Facade'''

	def __init__(self):
		self.washing = Washing()
		self.rinsing = Rinsing()
		self.spinning = Spinning()

	def startWashing(self):
		self.washing.wash()
		self.rinsing.rinse()
		self.spinning.spin()

""" main method """
if __name__ == "__main__":

	washingMachine = WashingMachine()
	washingMachine.startWashing()
```

Преимущества
------------

1.  Изоляция: мы можем легко изолировать наш код от сложности подсистемы.
2.  Процесс тестирования: Использование фасадного метода делает процесс тестирования сравнительно простым, поскольку он имеет удобные методы для обычных задач тестирования.
3.  Слабая связь: наличие слабой связи между клиентами и подсистемами.

Недостатки
----------

1.  Изменения в методах: Поскольку мы знаем, что в методе фасада последующие методы привязаны к слою фасада, и любое изменение в последующем методе может привести к изменению слоя фасада, которое не является благоприятным.
2.  Дорогостоящий процесс: установка фасадного метода в нашем приложении обходится недешево для обеспечения надежности системы.
3.  Нарушение правил: всегда существует опасение нарушения построения фасадного слоя.

 Применимость
------------

1.  Обеспечение простого интерфейса: Одним из наиболее важных применений фасадного метода является то, что он используется всякий раз, когда вы хотите предоставить простой интерфейс сложной подсистеме
2.  Разделение на слои: он используется, когда мы хотим придать подсистеме уникальную структуру путем разделения их на слои. Это также приводит к ослаблению связи между клиентами и подсистемой.