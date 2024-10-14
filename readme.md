# Фреймворк для формирования продуктовых требований к программным продуктам
## Введение
### Проблема
Сложно нормально описать все требования в плоском документе 
### Решение
Разделить описание на Представления (Views) и Фичи (Features). Применить подход ".. as Code" для формирования продуктовых требований к программным продуктам.
### Подход
1. Каждое представлени или фича описывается в отдельном файле;
2. В качестве языка описания используется YAML по правилами, содержащимся в данном руководстве;
3. Все файлы и изменения сохраняются в системе контроля версий;

### Пример
Пример описания страницы с формой авторизации.
~~~
view:
type: window
name: login_form_view
permissions: ["unauthorized"]
children:
  # Header
  - block:
    name: header
    text: Авторизаия
  # Form
  - block:
    name: login_form
    children:
    # header
      - text: 'Пожалуйса введите свои логин и пароль'
      - block:
        # userName_input
        name: userName_input
        children:
          - text: 'Логин'
          - input:
            tyoe: textField
            name: userName_input
      # password_input
      - block:
        name: password_input
        children:
          - text: 'Пароль'
          - input:
            tyoe: textField 
            name: password_input
      # show_password_switch
      - block:
        name: show_password_switch
        children:
          - text: 'Показать пароль'
          - input:
            type: checkbox
            name: show_password
            features:
              - *show_password_feature
      # submit_button
      - block:
        name: submit_button
        children:
          - text: 'Войти'
          - button:
            name: login_button
            features:
              - event:
                trigger: onClick
                action: login_feature
      # forgot_password_link
      - block:
        name: forgot_password_link
        children:
          - button:
            name: forgot_password_link
            features:
              - event:
                trigger: onClick
                action: go forgot_password_view

# Inline feature
features:
  - &show_password_feature
    feature:
      name: show_password_feature
      input: parent
      type: bool
      events:
        - event:
            trigger: show_password в положении true
            action: Показать пароль в поле password_input
        - event:
            trigger: show_password в положении false
            action: Обфусировать пароль в поле password_input          

~~~

## Модель
### Класс View (абстрактный)
Представления – это абстрактный класс, отвечающий за окна различных типов, в которых содержатся элементы интерфейса.
#### Свойства
_**name** (String)_  
Строчный идентификатор представления  
  
_**permissions** (Array of String)_  
Массив типов пользовтелей, которые имеют доступ к представлению  
  
_**events** (Array of Event)_  
Действия, которые могут совершаться с данным представлением. Может содержать  идентификатор feature,  
  
_**children** (Array of View)_  
Массив вложенных представлений  

### Производные от View
#### Window 
Используется для обозначения представления верхнего уровня - окна браузера.
#### Tab
Используется для описания содержимого закладок
#### Overlay
Используется для описания содержимого всплывающего окна. Моэет быть как диалогом, так и сайдбаром.
#### Block
Используется для объединения элементов в логические блоки.

### Класс Element (абстрактный)
Используется для обозначения компонентов пользовательского интерфейса с котороыми пользователь взаимодействует на экране.

### Производные от Element
#### Text
#### Table
#### Button
#### Input
#### DropDown
#### Image
#### Video

#### Свойства
_**name** (String)_  
Строчный идентификатор представления  
_**events** (Array of Event)_  
Строчный идентификатор представления  

### Класс Event
#### Свойства
_**trigger** (String)_  
_**action** (String)_   



1. В класс View входят Window, Page, Tabs, Overlay
2. Подкласс View входят Elements
3. В подкласс Elements входят  и логический подкласс Block
~~~
name: string
userTypeAccess: array
children: array
events: array
~~~
### Фичи
~~~
feature:
  name: 
  inputs:
  events:
  errors:
~~~
