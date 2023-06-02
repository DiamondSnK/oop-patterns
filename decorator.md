# ПАТТЕРН ДЕКОРАТОР(Decorator)
***
## Авторы:

Ермоленко Марина Викторовна  
Теплов Алексей Вячеславович
***
**Декоратор** — представляет структурный шаблон проектирования, который позволяет динамически подключать к объекту дополнительную функциональность.  
Для определения нового функционала в классах нередко используется наследование. Декораторы же предоставляет наследованию более гибкую альтернативу, поскольку позволяют динамически в процессе выполнения определять новые возможности у объектов.   
![](https://avatars.mds.yandex.net/i?id=5164a680d7f7a16695a7f451fcc49a2de9d780bb-9068918-images-thumbs&n=13)
***
# Проблема

Представьте, что вы работаете над библиотекой уведомлений, которая позволяет другим программам уведомлять своих пользователей о важных событиях.

Первоначальная версия библиотеки была основана на Notifier классе, в котором было всего несколько полей, конструктор и единственный send метод. Метод может принимать аргумент message от клиента и отправлять сообщение в список электронных писем, которые были переданы уведомителю через его конструктор. Стороннее приложение, выступавшее в роли клиента, должно было создать и настроить объект notifier один раз, а затем использовать его каждый раз, когда происходило что-то важное.
Программа могла бы использовать класс notifier для отправки уведомлений о важных событиях на заранее определенный набор электронных писем.
В какой-то момент вы понимаете, что пользователи библиотеки ожидают большего, чем просто уведомления по электронной почте. Многие из них хотели бы получать SMS-сообщения о критических проблемах. Другие хотели бы получать уведомления на Facebook, и, конечно же, корпоративные пользователи хотели бы получать уведомления Slack.
Насколько это может быть сложно? Вы расширили Notifier класс и поместили дополнительные методы уведомления в новые подклассы. Теперь клиент должен был создать экземпляр нужного класса уведомлений и использовать его для всех дальнейших уведомлений.

Но затем кто-то резонно спросил вас: “Почему вы не можете использовать несколько типов уведомлений одновременно? Если в вашем доме пожар, вы, вероятно, захотите, чтобы вас проинформировали по всем каналам ”.

Вы пытались решить эту проблему, создав специальные подклассы, которые объединили несколько методов уведомления в рамках одного класса. Однако быстро стало очевидно, что такой подход сильно раздул бы код, причем не только библиотечный, но и клиентский.

Комбинаторный взрыв подклассов.

Вам нужно найти какой-то другой способ структурирования классов уведомлений, чтобы их количество случайно не побило какой-нибудь рекорд Гиннесса.
 
***
  # Решение
  Расширение класса - это первое, что приходит на ум, когда вам нужно изменить поведение объекта. Однако у наследования есть несколько серьезных предостережений, о которых вы должны знать.

* Наследование является статичным. Вы не можете изменить поведение существующего объекта во время выполнения. Вы можете заменить весь объект другим объектом, созданным из другого подкласса, только целиком.
* Подклассы могут иметь только один родительский класс. В большинстве языков наследование не позволяет классу наследовать поведение нескольких классов одновременно.
Один из способов преодолеть эти предостережения - использовать агрегацию или композицию вместо наследования. Обе альтернативы работают почти одинаково: один объект имеет ссылку на другой и делегирует ему некоторую работу, тогда как при наследовании сам объект способен выполнять эту работу, наследуя поведение от своего суперкласса.

С помощью этого нового подхода вы можете легко заменить связанный объект “помощник” другим, изменяя поведение контейнера во время выполнения. Объект может использовать поведение различных классов, имея ссылки на множество объектов и делегируя им все виды работы.   
Агрегирование / композиция - ключевой принцип, лежащий в основе многих шаблонов дизайна, включая Decorator. На этой ноте давайте вернемся к обсуждению шаблона.

Wrapper - альтернативное название шаблона декоратора, которое четко выражает основную идею шаблона. “Оболочка” - это объект, который может быть связан с некоторым “целевым” объектом. Оболочка содержит тот же набор методов, что и целевой объект, и делегирует ему все запросы, которые он получает. Однако оболочка может изменить результат, выполнив что-либо до или после передачи запроса целевому объекту.

Когда простая оболочка становится настоящим декоратором? Как я уже упоминал, оболочка реализует тот же интерфейс, что и обернутый объект. Вот почему с точки зрения клиента эти объекты идентичны. Сделайте так, чтобы поле ссылки оболочки принимало любой объект, который следует за этим интерфейсом. Это позволит вам охватить объект несколькими оболочками, добавив к нему объединенное поведение всех оболочек.

В нашем примере с уведомлениями давайте оставим простое поведение уведомления по электронной почте внутри базового Notifier класса, но превратим все другие методы уведомления в декораторы.

Клиентский код должен был бы обернуть базовый объект notifier в набор декораторов, соответствующих предпочтениям клиента. Результирующие объекты будут структурированы в виде стека.

Последним декоратором в стеке будет объект, с которым на самом деле работает клиент. Поскольку все декораторы реализуют тот же интерфейс, что и базовый уведомитель, остальной клиентский код не будет заботиться о том, работает ли он с “чистым” объектом notifier или с оформленным.

Мы могли бы применить тот же подход к другим способам поведения, таким как форматирование сообщений или составление списка получателей. Клиент может украсить объект любыми пользовательскими декораторами, если они соответствуют тому же интерфейсу, что и другие.
  ***
  # Пример: 
  ```class AbstractBlock:
  """ Абстрактный блок
  """
  def draw(self):
    raise NotImplementedError();

class TerminatorBlock(AbstractBlock):
  """ Терминальный блок (начало/конец, вход/выход)
  """
  def draw(self):
    print "Terminator block drawing ... "

class ProcessBlock(AbstractBlock):
  """ Блок - процесс (один или несколько операторов)  
  """
  def draw(self):
    print "Process block drawing ... "

class AbstractBlockDecorator(AbstractBlock):
  """ Абстракный декоратор блоков
  """
  def __init__(self, decoratee):
    # _decoratee - ссылка на декорируемый объект
    self._decoratee = decoratee
  
  def draw(self):
    self._decoratee.draw()

class LabelBlockDecorator(AbstractBlockDecorator):
  """ Декорирует блок текстовой меткой
  """
  def __init__(self, decoratee, label):
    self._decoratee = decoratee
    self._label = label
  
  def draw(self):
    AbstractBlockDecorator.draw(self)
    self._drawLabel()
  
  def _drawLabel(self):
    print " ... drawing label " + self._label

class BorderBlockDecorator(AbstractBlockDecorator):
  """ Декорирует блок специальной рамкой
  """
  def __init__(self, decoratee, borderWidth):
    self._decoratee = decoratee
    self._borderWidth = borderWidth
  
  def draw(self):
    AbstractBlockDecorator.draw(self)
    self._drawBorder()
  
  def _drawBorder(self):
    print " ... drawing border with width " + str(self._borderWidth)

# терминальный блок
tBlock = TerminatorBlock()
# блок - процесс
pBlock = ProcessBlock()

# Применим LabelDecorator к терминальному блоку
labelDecorator = LabelBlockDecorator(tBlock, "Label222")

# Применим BorderDecorator к терминальному блоку, после применения LabelDecorator
borderDecorator1 = BorderBlockDecorator(labelDecorator, 22)

# Применим BorderDecorator к блоку - процессу
borderDecorator2 = BorderBlockDecorator(pBlock, 22)

labelDecorator.draw()
borderDecorator1.draw()
borderDecorator2.draw()

```
 # Преимущества и недостатки

### Преимущества

* Вы можете расширить поведение объекта, не создавая новый подкласс.
* Вы можете добавлять или удалять обязанности из объекта во время выполнения.
* Вы можете комбинировать несколько вариантов поведения, заключая объект в несколько декораторов.
* Принцип единой ответственности. Вы можете разделить монолитный класс, который реализует множество возможных вариантов поведения, на несколько меньших классов.

### Недостатки

* Сложно удалить определенную оболочку из стека оберток.
* Сложно реализовать декоратор таким образом, чтобы его поведение не зависело от порядка в стеке декораторов.
* Исходный код конфигурации слоев может выглядеть довольно уродливо.