# Фреймворк для формирования продуктовых требований к программным продуктам
## Содержание
1. [Представления](./views/views.md)
2. [Элементы](./views/elements.md)
3. [Функциональные блоки](./features/features.md)

## Введение
### Проблема
Сложно нормально описать все требования в плоском документе 
### Решение
Разделить описание на Представления (Views) и функциональные блоки (Features). Применить подход ".. as Code" для формирования продуктовых требований к программным продуктам.
### Подход
1. Каждое представлени или фича описывается в отдельном файле;
2. В качестве языка описания используется YAML по правилами, содержащимся в данном руководстве;
3. Все файлы и изменения сохраняются в системе контроля версий;

### Модель
Для отображения структуры используются представления (Views), которые содержат элементы интерфейса (Elements). Для обозначения функциональных блоков используются фичи (Features).

### Пример
Пример описания страницы с формой авторизации.
~~~
window:
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
      # userName_input
      - block:
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
      inputs: 
        #  Состояние чекбокса
        - input:
          name: show_password_state 
          type: bool
      events:
        - event:
          trigger: show_password_state == true
          action: Показать пароль в поле password_input
        - event:
          trigger: show_password_state == false
          action: Обфусировать пароль в поле password_input 

~~~

Следующая статья: [Представления](./views/views.md)