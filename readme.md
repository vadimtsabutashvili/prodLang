# ProdLang
ProdLang — это фреймворк для формализованного описания продуктовых требований к программным продуктам с использованием подхода "... as Code". Он упрощает создание требований и последующее управление ими.
## Содержание
1. Введение
2. [Представления](./views/views.md)
3. [Элементы](./views/elements.md)
4. [Функциональные блоки](./features/features.md)

## Введение
### Проблема
Традиционные способы описания продуктовых требований в виде плоских документов создают несколько проблем:

* Сложности с четким разделением рабочей логики и интерфейса, а также с установлением связей между ними;
* Затрудненная навигация по крупным документам, что усложняет поиск нужной информации;
* Невозможность удобного переиспользования и масштабирования отдельных блоков требований, что приводит к дублированию и усложняет их поддержку;
* Задачи на разработку ставятся в отрыве от остальных требований.

### Решение
В ProdLang предлагается подход, при котором требования к интерфейсу и функциональности делятся на две категории:

* Представления (Views): описывают структуру и элементы пользовательского интерфейса.
* Функциональные блоки (Features): описывают логику и функциональность, привязанную к элементам интерфейса.

Применяемый подход "... as Code" позволяет описывать продуктовые требования как код, используя YAML, что позволяет:

* Структурированное описание;
* Возможность версионирования в системах контроля версий (Git и т.д.);
* Улучшенную читаемость и управляемость требований.

## Сравнение с User Story

Использование ProdLang не заменяет User Stories, а дополняет их, создавая эффективную связь между менеджерами по продукту и разработчиками. Этот подход служит мостом между бизнес-требованиями и технической реализацией, превращая высокоуровневую User Story в понятное и структурированное задание для разработки.  

Менеджеры могут продолжать использовать User Stories как основу для постановки задач. Эти истории задают направление и описывают ожидаемый результат с точки зрения пользователя. Затем с помощью ProdLang эти задачи преобразуются в детализированные спецификации, которые содержат четкие инструкции по реализации, структуре интерфейса, логике поведения и обработке ошибок.  

Таким образом, ProdLang делает процесс разработки более прозрачным, снижает количество ошибок и ускоряет прохождение задач, обеспечивая полное взаимопонимание между всеми участниками проекта.  

### Пример
Пример описания страницы с формой авторизации.
~~~
window:
  name: login_form_view
  description: 
    "
    Для доступа к персональному разделу необходимо авторизоваться с помощью логина (адреса электронной почты) и пароля, который пользователь получает после регистрации.
    "
  permissions: ["unauthorized"]
  children:
    # Заrоловок блока
    - block:
      name: header
      text: Авторизаия

    # Форма авторизации
    - block:
      name: login_form
      children:
        - text: 'Пожалуйста введите свои логин и пароль'
        - block:
          name: userName_input
          children:
            - text: 'Логин'
            - inputField:
              name: userName_input
        - block:
          name: password_input
          children:
            - text: 'Пароль'
            - inputField:
              name: password_input
        # Возможность покаать/скрыть пароль
        - block:
          name: show_password_switch
          children:
            - text: 'Показать пароль'
            - checkbox:
              name: show_password_checkbox
              features:
                - *show_password_feature
        # Кнопка отправки формы
        - block:
          name: submit_button
          children:
            - text: 'Войти'
            - button:
              name: login_button
              feature:
                - event:
                  trigger: onClick
                  action: run login_feature
                  output:
                    cases:
                      - case: login_feature.output.cases.case == success
                        action: go main_view where authorized_user.state = ["authorized"]
                      - case: login_feature output == error
                        action: show login_error_view 
                         

        # forgot_password_link
        - block:
          name: forgot_password_link
          children:
            - link:
              name: forgot_password_link
              feature:
                - event:
                  trigger: onClick
                  action: go forgot_password_view

# Встроеннаый функциональной блок
features:
  - &show_password_feature
    feature:
      name: show_password_feature
      inputs: 
        #  Состояние чекбокса
        - input:
          name: show_password_checkbox_state 
      events:
        - event:
          trigger: show_password_state == true
          action: Показать пароль в поле password_input
        - event:
          trigger: show_password_state == false
          action: Обфусцировать пароль в поле password_input 

~~~

Следующая статья: [Представления](./views/views.md)