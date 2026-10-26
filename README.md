# Search Service

Go-сервис поиска с конфигурируемыми схемами индексов. **Сейчас здесь намеренно нет Go-кода и `go.mod`.** Реализация начнётся после утверждения архитектурной задачи.

## Три репозитория

- Архитектура/Obsidian: `https://github.com/YOUR_GITHUB_OWNER/search-service-architecture`
- Приложение и CI: этот репозиторий
- Окружение/Dev Container: `https://github.com/YOUR_GITHUB_OWNER/search-service-devcontainer` — подключается как Git submodule в `.devcontainer/`

Правила Codex описаны в [AGENTS.md](AGENTS.md).

## Привязка инфраструктуры

После публикации репозитория окружения из корня приложения:

```bash
git submodule add https://github.com/YOUR_GITHUB_OWNER/search-service-devcontainer.git .devcontainer
git add .gitmodules .devcontainer
git commit -m "chore: pin devcontainer infrastructure"
```

При новом клонировании:

```bash
git clone --recurse-submodules https://github.com/YOUR_GITHUB_OWNER/search-service.git
cd search-service
cp .devcontainer/.env.example .devcontainer/.env
# Открыть проект в VS Code -> Dev Containers: Reopen in Container
```

## Проверки качества

Workflow `.github/workflows/quality.yml` в этой фазе успешно выполняет bootstrap-проверку. При появлении `go.mod` будут включены `gofmt`, `go mod tidy -diff`, `go vet`, `golangci-lint`, тесты с `-race` и сбор отчёта покрытия.

Значение GitHub repository variable `MIN_COVERAGE` по умолчанию `0` — только до появления реального baseline. После первой реализации согласуем положительный порог.

Команды для локальной проверки после появления Go-модуля:

```bash
gofmt -l .
go vet ./...
golangci-lint run ./...
go test -race -covermode=atomic -coverprofile=coverage.out ./...
go tool cover -func=coverage.out
```

В этом репозитории нет production-deploy, Kafka, inference, RAG и Go-кода.
