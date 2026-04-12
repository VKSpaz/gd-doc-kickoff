# gd-doc-kickoff

Универсальный skill для проведения геймдизайн-интервью и сборки production-ready спецификации фичи в Markdown.

Подходит для двух сценариев:
- у вас уже есть концепт и нужно быстро собрать качественный GD-док;
- концепта нет, и нужно сначала сгенерировать 2-3 рабочих направления идеи, а потом перейти к спецификации.

## Для чего нужен

Skill ведет пользователя через структурированный диалог и помогает:
- зафиксировать цели, scope, логику, UX, риски, метрики, rollout;
- принудительно собрать rationale по ключевым решениям (зачем/почему);
- добавить формулы, баланс-переменные и localization keys;
- автоматически отфильтровать F2P-вопросы для premium-игр.

## Совместимость

Skill сделан модель-агностичным и рассчитан на:
- Codex
- Kilokode
- Claude
- DeepSeek

В папке `agents/` есть отдельные profile-файлы:
- `openai.yaml`
- `codex.yaml`
- `kilokode.yaml`
- `claude.yaml`
- `deepseek.yaml`

Если в вашей среде нет автоподхвата `agents/*.yaml`, используйте `SKILL.md` как основной источник инструкций.

## Установка из Git (целиком)

Идея простая: клонируете репозиторий полностью в папку skills вашего окружения. Ничего вручную по файлам раскладывать не нужно.

### Вариант A: Codex (Windows, PowerShell)

```powershell
git clone https://github.com/VKSpaz/gd-doc-kickoff.git "$env:USERPROFILE\.codex\skills\gd-doc-kickoff"
```

### Вариант B: Codex (macOS/Linux)

```bash
git clone https://github.com/VKSpaz/gd-doc-kickoff.git "$HOME/.codex/skills/gd-doc-kickoff"
```

### Вариант C: Любое другое окружение

1. Клонируйте репозиторий в локальную папку:
```bash
git clone https://github.com/VKSpaz/gd-doc-kickoff.git
```
2. Подключите эту папку как локальный skill/prompt-пакет в вашем инструменте.
3. Если есть выбор entry-point, указывайте `SKILL.md`.

После установки перезапустите клиент ассистента или обновите список skills.

## Как работает

1. Onboarding:
   - проверяет, есть ли у пользователя готовый концепт;
   - если нет, запускает ideation-поток (`references/idea-discovery-ru.md`) и собирает 3 направления идей.
2. Concept decomposition:
   - через вопросы раскладывает концепт на структуру фич (`references/feature-decomposition-ru.md`);
   - сохраняет `docs/project-concept.md` и `docs/feature-index.md`.
3. Context discovery:
   - ищет контекст каскадом:
     - путь от пользователя;
     - `docs/project-concept.md`;
     - `ai-docs/feature-index.md`;
     - `docs/feature-index.md`;
     - `docs/architecture.md`;
     - `README.md`.
4. Distribution gate:
   - определяет `f2p` / `premium` / `hybrid`;
   - в `premium` режиме не задает F2P-специфику без явной необходимости.
5. Interview + draft:
   - проходит обязательные и контекстные блоки;
   - генерирует итоговый `.md`.

## Куда сохраняется итоговый файл

По умолчанию итоговый документ сохраняется в:
- `<project_root>/docs/`

Файл именуется так:
- `YYYY-MM-DD_<feature-slug>_gd-spec.md`

Если `docs/` не существует, папка создается автоматически.

Дополнительно в `docs/` поддерживаются:
- `project-concept.md`
- `feature-index.md`

## Важные правила skill

- Сильный акцент на вопросы "зачем/почему".
- Никаких советов по именам классов/методов и написанию кода.
- Формулы/баланс-переменные/локализационные ключи обязательны, когда релевантны фиче.
- Для balance vars и localization keys используется `snake_case`.

## Структура репозитория

- `SKILL.md` — основная логика skill.
- `references/gdd-core-template.md` — каркас итогового документа.
- `references/question-bank-ru.md` — обязательные и контекстные вопросы.
- `references/context-triggers.md` — триггеры для условных блоков.
- `references/idea-discovery-ru.md` — сценарий генерации концепта, если идеи нет.
- `references/feature-decomposition-ru.md` — сценарий декомпозиции концепта в фичи.
- `agents/*.yaml` — profile-файлы для разных ассистентов.
