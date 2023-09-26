# Автоматизировать рутинные действия в задачах

Прохождение новым сотрудником испытательного срока связано с рутинными действиями, трекинг которых можно автоматизировать.

## Настройте автоматическое назначение окончания испытательного срока

Чтобы дата окончания испытательного срока выставлялась автоматически, создайте триггер на событие создания задачи:

1. На странице очереди сотрудников `Employment Queue` в правом верхнем углу нажмите ![](../_assets/tracker/svg/queue-settings.svg) **{{ ui-key.startrek.ui_components_PageQueue_header.settings }}**.
1. На панели слева выберите **{{ ui-key.startrek.blocks-desktop_b-page-queue-admin.group-title--automatization }}** → **{{ ui-key.startrek.blocks-desktop_b-page-queue-admin-tab_type_triggers.title }}** и нажмите кнопку **{{ ui-key.startrek.blocks-desktop_b-page-queue-admin-tab_type_triggers.button-create }}**.
1. В поле **{{ ui-key.startrek.blocks-desktop_b-page-queue-admin-tab_type_trigger-editor.label_name }}** введите название триггера, например `probation_setup`.
1. В блоке **{{ ui-key.startrek.blocks-desktop_b-page-queue-admin-tab_type_trigger-editor.label_conditions }}** выберите **{{ ui-key.startrek-backend.fields.issue.fields.system }}**  → **{{ ui-key.startrek-backend.fields.issue.type-key-value }}**.
1. В появившемся поле выберите опцию **{{ ui-key.startrek-backend.messages.trigger.condition.type.fieldEquals }}**, а в новом поле рядом выберите опцию **Испытательный срок** (тип задачи, созданный на предыдущем этапе).
1. Добавьте еще одно условие: в блоке **{{ ui-key.startrek.blocks-desktop_b-page-queue-admin-tab_type_trigger-editor.label_conditions }}** выберите **{{ ui-key.startrek.blocks-desktop_trigger-condition.condition-type--event }}** и в появившемся поле выберите опцию **{{ ui-key.startrek-backend.events.event.IssueCreated }}**.
1. В блоке **{{ ui-key.startrek.blocks-desktop_b-page-queue-admin-tab_type_trigger-editor.label_actions }}** в поле **{{ ui-key.startrek.blocks-desktop_b-page-queue-admin-tab_type_trigger-editor.action_add-action }}** выберите **{{ ui-key.startrek.blocks-desktop_trigger-action.select-action--formula }}**.
1. В появившемся поле введите формулу:
	```
	now()+3M
	```

## Настройте автоматическое создание чеклиста

Чтобы не упустить из внимания важные моменты в процессе введения нового сотрудника в рабочий режим, добавьте в действия триггера создание чеклиста:

1. В блоке **{{ ui-key.startrek.blocks-desktop_b-page-queue-admin-tab_type_trigger-editor.label_actions }}** в поле **{{ ui-key.startrek.blocks-desktop_b-page-queue-admin-tab_type_trigger-editor.action_add-action }}** выберите **{{ ui-key.startrek.blocks-desktop_trigger-action.select-action--checklist }}**.
1. В появившейся форме нажмите кнопку **{{ ui-key.startrek.ui_components_Checklist.new-item-button-caption }}** и введите описание первого пункта, например `Назначить куратора`.
1. Аналогично добавьте еще пункты в чеклист.
1. Чтобы сохранить триггер, нажмите кнопку **{{ ui-key.startrek.blocks-desktop_b-page-queue-admin-tab_type_trigger-editor.action_create }}**.

## Настройте автоматическое изменение статуса задачи

Чтобы перевести задачу в следующий статус по факту заполнения чеклиста, создайте новый триггер:

1. Нажмите кнопку **{{ ui-key.startrek.blocks-desktop_b-page-queue-admin-tab_type_triggers.button-create }}**.
1. В поле **{{ ui-key.startrek.blocks-desktop_b-page-queue-admin-tab_type_trigger-editor.label_name }}** введите название триггера, например `probation_checklist`.
1. В качастве условий срабатывания триггера выберите **{{ ui-key.startrek-backend.messages.trigger.condition.field.checklist }}** и **{{ ui-key.startrek-backend.fields.issue.type-key-value }}** → **{{ ui-key.startrek-backend.messages.trigger.condition.type.fieldEquals }}** → **Испытательный срок**.
1. В качестве действия триггера выберите **{{ ui-key.startrek-backend.types.types.trigger.action.transition }}** и укажите статус, например **В работе**.

   {% note warning %}

   Статус задачи не изменится, если при переходе в указанный статус есть обязательные для заполнения поля. 
	Например, статус **{{ ui-key.startrek-backend.applinks.otrs.status.closed }}** предусматривает обязательный комментарий, поэтому его нельзя использовать в данном действии.

   {% endnote %}

1. Сохраните триггер.

## Проверьте действие триггеров

1. На странице очереди новых сотрудников `Employment Queue` выберите задачу типа `Новый сотрудник`.
1. Измените статус на **В работу**.
1. Убедитесь, что в очереди появились все необходимые подзадачи.
1. Зайдите в подзадачу типа `Испытательный срок` и убедитесь, что:
	* в поле **{{ ui-key.startrek.blocks-desktop_st-gantt-issue-update-modal.label.due-date }}** указана дата через три месяца;
	* присутствует блок **{{ ui-key.startrek-backend.fields.issue.checklistItems }}** с необходимыми пунктами.
1. Активируйте все пункты чеклиста и перезагрузите страницу. Убедитесь, что статус задачи изменился.
