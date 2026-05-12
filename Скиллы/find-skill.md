---
name: find-skill
description: Meta-skill that finds, recommends, or creates the right skill for a task or agent. Use when the user asks "find skills for X", "what skills does this agent need", or wants to discover capabilities they don't have yet.
tools: Read, Write, Edit, Bash, Glob, Grep, WebSearch
---

# find-skill

Универсальный поиск и подбор скиллов под задачу или агента. Заменяет ручной поход на биржу скиллов.

## Когда использовать

- Пользователь спрашивает: «какие скиллы нужны агенту X»
- Пользователь говорит: «найди скиллы для маркетинга / продаж / финансов»
- Пользователь добавляет нового агента и нужно его прокачать
- Пользователь хочет апгрейднуть агента с уровня 1 на уровень 2

## Workflow

### Шаг 1: Понять задачу
- Что должен делать агент или задача?
- На каком уровне сейчас агент (1/2/3)?
- Какие у него уже подключены скиллы (читаем frontmatter в `.claude/agents/`)
- Какие пробелы

### Шаг 2: Поиск в существующих библиотеках
1. **Локальная библиотека проекта**: `.claude/skills/`
2. **Глобальная библиотека**: `~/.claude/skills/`
3. **Установленные плагины**: `~/.claude/plugins/marketplaces/*/plugins/*/skills/`

```bash
ls .claude/skills/ 2>/dev/null
ls ~/.claude/skills/ 2>/dev/null
find ~/.claude/plugins -name "SKILL.md" 2>/dev/null
```

### Шаг 3: Если нашли подходящие — подключить
Обновить frontmatter агента:
```yaml
skills: skill-name-1, skill-name-2
level: 2
```

### Шаг 4: Если нет — создать новый скилл
Создать файл в нужной папке:
- Локально (под проект): `.claude/skills/<skill-name>/SKILL.md`
- Глобально (для всех проектов): `~/.claude/skills/<skill-name>/SKILL.md`

Структура SKILL.md:
```yaml
---
name: skill-name
description: Чёткое описание что делает и когда применять
tools: Read, Write, Bash, WebSearch
---

# Skill Name

Подробные инструкции для Claude как выполнять этот скилл.

## Когда использовать
...

## Workflow
...

## Примеры
...
```

### Шаг 5: Веб-поиск (опционально)
Если нужны идеи — `WebSearch` по запросу: «Claude Code skill <название задачи>».

### Шаг 6: Обновить карту скиллов
После любых изменений — обновить карту скиллов в базе знаний.

## Принципы привязки

**Глобально (`~/.claude/skills/`):**
- Универсальные мета-скиллы (find-skill, weekly-digest, отчётность)
- Скиллы под Claude Code и Obsidian
- То что используется в нескольких проектах

**Локально (`.claude/skills/`):**
- API под конкретный проект
- Бизнес-логика конкретной компании
- Уникальные интеграции (бот конкретного канала)

## Антипаттерны

- Не дублировать локально то что уже стоит глобально
- Не создавать скилл «на всякий случай» без задачи
- Не делать скилл расплывчатым («делает всё»)
- Не привязывать скилл к агенту без проверки frontmatter
