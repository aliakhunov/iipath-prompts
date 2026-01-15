# Context Transfer Protocol (Conversation Continuation)

## Purpose
Enable seamless continuation of work across multiple LLM conversations by generating a self-contained system prompt that preserves goals, decisions, roles, and artifacts when token limits are reached.

## When to use
Use this prompt when an ongoing dialogue approaches or hits the context/token limit and restarting the conversation without loss of progress, intent, or constraints is critical.

Typical scenarios include:
- long analytical or architectural discussions,
- multi-step writing or design work,
- iterative prompt engineering,
- extended learning or mentoring sessions.

## What this prompt does
The prompt forces the model to compile a **self-executing context capsule** that can be copied into a new chat and immediately resume work without re-explanation.

It explicitly:
- reconstructs role, tone, and constraints,
- synchronizes goals and current focus,
- preserves named entities, decisions, and artifacts,
- defines a clear breakpoint,
- triggers the next required action without greetings or warm-up.

## Key constraints
- The generated context is written **from the user to the model**, not as a summary.
- Output format is **a single code block only**.
- The resulting prompt is designed to be pasted directly into a new conversation without modification.

## Output format
- One system-style prompt
- Strict internal structure:
  - Role and style activation
  - Goals and current focus
  - Knowledge base and artifacts
  - Breakpoint definition
  - Immediate action command

## Notes
- This is a workflow/meta prompt, not a task-solving prompt.
- It improves reliability and continuity when working at scale with LLMs.
- Especially useful in combination with behavioral or verification protocols.

---

## Prompt
```text
# СИСТЕМНАЯ ДИРЕКТИВА: ГЕНЕРАЦИЯ САМОИСПОЛНЯЮЩЕГОСЯ КОНТЕКСТА

Мы достигли лимита токенов. Необходимо создать "капсулу времени" для переноса работы в новый диалог.

**Твоя задача:**
Скомпилировать исчерпывающий системный промпт, который я скопирую и вставлю в новый чат.

**КРИТИЧЕСКИЕ УСЛОВИЯ ГЕНЕРАЦИИ:**
1.  **Имитация голоса:** Весь сгенерированный текст должен быть написан **от моего лица (Пользователя) к Нейросети**. Не используй фразы "Вот ваш контекст". Пиши: "Твоя задача...", "Мы остановились на...".
2.  **Структура данных:** Ты обязан сохранить глубину контекста, используя структуру, описанную ниже.
3.  **Формат:** ТОЛЬКО один блок кода (Code Block).

---
**СТРУКТУРА ПРОМПТА, КОТОРЫЙ ТЫ ДОЛЖЕН НАПИСАТЬ:**

1.  **АКТИВАЦИЯ РОЛИ И СТИЛЯ:**
    *   Начни с команды: *"Прими роль [Твоя текущая роль]. Сохраняй стиль общения: [Твой текущий стиль/Тон]."*
    *   Укажи специфические ограничения (формат вывода, запрещенные темы и т.д.).

2.  **СИНХРОНИЗАЦИЯ ЦЕЛЕЙ:**
    *   **Глобальная цель:** Ради чего мы всё это делаем?
    *   **Текущий фокус:** Какую подзадачу мы решаем прямо сейчас?

3.  **БАЗА ЗНАНИЙ И АРТЕФАКТЫ:**
    *   **Ключевые сущности:** (Имена, термины, переменные, названия проектов — всё, что имеет имя собственное в нашем диалоге).
    *   **Утвержденные тезисы:** (Список решений, которые мы уже приняли и зафиксировали, чтобы не обсуждать их заново).
    *   **Состояние артефактов:** (Что мы создавали? Код, главы книги, таблицы? В каком они состоянии? Опиши их структуру или суть, чтобы не потерять логику).

4.  **ТОЧКА РАЗРЫВА:**
    *   Цитата или описание последнего момента перед остановкой.
    *   Что конкретно осталось незавершенным?

5.  **ИМПЕРАТИВНАЯ КОМАНДА:**
    *   Закончи промпт фразой: *"Изучив этот контекст, НЕМЕДЛЕННО приступи к выполнению следующего шага: [Опиши конкретное действие, которое нужно сделать первым в новом чате]. Не трать токены на приветствия, просто выдай результат."*

---
Начинай генерацию блока кода прямо сейчас.
```
