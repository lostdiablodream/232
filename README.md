собери вот эти скиллы, исследуй их, укрась стилем интерфейс дашборда. """Начну с поиска готовых skills/rules для веб-дизайна и актуальных практик.Нашёл несколько репозиториев с готовыми skills для веб-дизайна. Для вашего погодного дашборда я бы собрал набор из пяти частей. Все ссылки взяты из выдачи поиска.

## Основные skills (дизайн и вёрстка)

1. **anthropics/skills → frontend-design.** Это официальный skill, который учит избегать шаблонной «AI-эстетики»: продуманные шрифты, чёткая типографическая шкала, сдержанная анимация только там, где она оправдана.
   - https://github.com/anthropics/skills/blob/main/skills/frontend-design/SKILL.md
   - Raw: `https://raw.githubusercontent.com/anthropics/skills/main/skills/frontend-design/SKILL.md`
2. **addyosmani/agent-skills → frontend-ui-engineering.** Production-качество, доступность, адаптивность; есть таблица признаков «AI-вида» (лишние градиенты, везде rounded-2xl, тяжёлые тени) и рекомендация сначала составить короткий design contract. Репозиторий недавно обновлялся.
   - https://github.com/addyosmani/agent-skills/blob/main/skills/frontend-ui-engineering/SKILL.md
3. **PracticalSwan/agent-skills → frontend-design (v2.0, 2026-09-08).** Критерий «лучшего» здесь не один стиль, а соответствие контексту; доступность и корректная работа заданы как обязательные условия, а skill явно упоминает дашборды.
   - https://github.com/PracticalSwan/agent-skills/blob/main/frontend-design/SKILL.md
4. **joshuadavidthomas/agent-skills → frontend-design-principles.** Ориентирован на дашборды и админки; задаёт решения по типографике, цвету, раскладке и именованию CSS-токенов вместо типовых умолчаний, а для продуктовых и data-интерфейсов направляет в app.md.
   - https://github.com/joshuadavidthomas/agent-skills/blob/main/frontend-design-principles/SKILL.md
   - Этот skill ссылается на дополнительные файлы (app.md, marketing.md), поэтому клонируйте репозиторий целиком.

## Аудит интерфейса

- **vercel-labs/agent-skills → web-design-guidelines.** Проверяет код на соответствие Web Interface Guidelines и выдаёт замечания в формате file:line.
  - https://github.com/vercel-labs/agent-skills/tree/HEAD/skills/web-design-guidelines
  - Нюанс: skill каждый раз подтягивает свежие правила по внешнему URL, а не хранит их внутри. Для офлайн-архива скачайте и этот файл:
  - `https://raw.githubusercontent.com/vercel-labs/web-interface-guidelines/main/command.md`

## Графики и данные (важно для погоды)

- **slb2248/ai-ux-skills → data-visualization.** Принципы дашбордов и графиков: контраст не ниже 4.5:1, текстовые альтернативы, таблицы данных, адаптивные графики.
  - https://skills.cat/skills/slb2248/ai-ux-skills/data-visualization
- **community-access/accessibility-agents → data-visualization-accessibility.** Доступность графиков: семантика, клавиатурная навигация, цвета, безопасные для дальтоников, понятные сводки для скринридеров.
  - https://skillselion.com/skills/community-access/accessibility-agents/data-visualization-accessibility (оттуда есть ссылка на GitHub)

## Свежие возможности CSS (на 2026 год)

Это не skill, а справочник, который можно положить в архив как заметку. В дайджестах web.dev Baseline отмечено, что функция CSS `contrast-color()` стала Baseline Newly available в апреле 2026, `:active-view-transition` стала Newly available в январе 2026, а двузначный синтаксис `display` стал Widely available. Перед использованием проверьте поддержку на web.dev.

- https://web.dev/blog/baseline-digest-apr-2026
- https://web.dev/blog/baseline-digest-may-2026
- https://web.dev/blog/baseline-digest-jan-2026

## Агрегаторы, чтобы взять больше

- https://github.com/finfin/awesome-frontend-skills — курируемый список frontend-skills, большинство ставится через `npx skills add`.
- https://github.com/flitzrrr/frontend-design-skills — 21 skill, установщик под Claude Code, Cursor, Windsurf, Copilot, Codex, Aider и другие.
- https://github.com/MengTo/Skills — коллекция skill-папок со SKILL.md и справочными файлами для дизайнеров и разработчиков.

## Как собрать архив локально

```bash
mkdir design-skills && cd design-skills

git clone --depth 1 https://github.com/anthropics/skills.git _anthropics
git clone --depth 1 https://github.com/addyosmani/agent-skills.git _addy
git clone --depth 1 https://github.com/PracticalSwan/agent-skills.git _swan
git clone --depth 1 https://github.com/joshuadavidthomas/agent-skills.git _josh
git clone --depth 1 https://github.com/vercel-labs/agent-skills.git _vercel

mkdir -p skills
cp -r _anthropics/skills/frontend-design              skills/frontend-design-anthropic
cp -r _addy/skills/frontend-ui-engineering            skills/
cp -r _swan/frontend-design                           skills/frontend-design-v2
cp -r _josh/frontend-design-principles                skills/
cp -r _vercel/skills/web-design-guidelines            skills/
curl -sL https://raw.githubusercontent.com/vercel-labs/web-interface-guidelines/main/command.md \
  -o skills/web-design-guidelines/command.md

rm -rf _anthropics _addy _swan _josh _vercel
zip -r design-skills.zip skills
```

Несколько skills можно подключать через `npx skills add owner/repo --skill name`, например: `npx skills add anthropics/skills@frontend-design`. Папку `skills/` кладут в `.claude/skills/` или `.agents/skills/`, либо в `~/.claude/skills/` для всех проектов. Для агентов без поддержки skills (Cursor, Aider) содержимое можно слить в `AGENTS.md`, `.cursorrules` или `.aider/conventions.md`.

## Что учесть

- Skills из сторонних репозиториев по сути инструкции для вашего агента. Перед включением в архив откройте каждый SKILL.md и убедитесь, что там нет лишних команд и внешних загрузок. Вендорить и фиксировать коммит лучше, чем подтягивать «последнюю версию».
- Несколько дизайн-skills одновременно могут противоречить друг другу (например, «будь смелым» против «следуй существующей дизайн-системе»). Держите один главный skill и пару вспомогательных.
- Я не открывал каждый файл целиком, только фрагменты из поиска. Актуальность и лицензии проверьте в самих репозиториях.

Если хотите, соберу для вас отдельный `weather-dashboard-design/SKILL.md`: токены цвета под погодные данные, правила для графиков температуры и осадков, доступность, адаптивность и чек-лист. Тогда общие skills будут работать вместе с ним. Для этого пришлите ссылку или код вашего текущего сайта."""
