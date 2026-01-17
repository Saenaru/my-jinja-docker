# StaticJinjaPlus Docker Port

Неофициальный порт для сборки Docker-образов [StaticJinjaPlus](https://github.com/MrDave/StaticJinjaPlus) — инструмента для компиляции Jinja2 шаблонов в статический HTML.

Этот репозиторий содержит только инструкции по сборке (Dockerfiles).

## Как собрать образ

В репозитории есть два файла:
* `Dockerfile.slim` — база python:3.11-slim (рекомендуется).
* `Dockerfile.ubuntu` — база ubuntu:22.04.

### Аргументы сборки (Build Args)

* `VERSION`: Версия тега (например, `0.1.1`) или название ветки (например, `main`).
* `REF_TYPE`: Тип ссылки. Используйте `tags` для релизов и `heads` для веток.
* `CHECKSUM`: SHA256 хэш архива с исходным кодом (обязательно для безопасности).

### Пример 1: Сборка стабильного релиза (0.1.1)

1. **Получите хэш версии:**
   ```bash
		curl -L [https://github.com/MrDave/StaticJinjaPlus/archive/refs/tags/0.1.1.tar.gz](https://github.com/MrDave/StaticJinjaPlus/archive/refs/tags/0.1.1.tar.gz) | sha256sum
	```

2. **Запустите сборку (Slim версия):**
   ```bash
docker build -f Dockerfile.slim \
  --build-arg REF_TYPE=tags \
  --build-arg VERSION=0.1.1 \
  --build-arg CHECKSUM=<ВАШ_ПОЛУЧЕННЫЙ_ХЭШ> \
  -t my-static-jinja:0.1.1-slim .
	```
	
### Пример 2: Сборка свежей версии (ветка main)
1.  **Получите хэш ветки main:**
    ```bash
    curl -L [https://github.com/MrDave/StaticJinjaPlus/archive/refs/heads/main.tar.gz](https://github.com/MrDave/StaticJinjaPlus/archive/refs/heads/main.tar.gz) | sha256sum
	```

2. **Запустите сборку (Ubuntu версия):**
   ```bash
docker build -f Dockerfile.ubuntu \
  --build-arg REF_TYPE=heads \
  --build-arg VERSION=main \
  --build-arg CHECKSUM=<ВАШ_ПОЛУЧЕННЫЙ_ХЭШ> \
  -t my-static-jinja:develop .
```

### Как использовать
   ```bash
	docker run --rm \
  -v "$(pwd)":/site \
  -w /site \
  my-static-jinja:0.1.1-slim \
  --srcpath ./templates \
  --outpath ./build
```

## Цели проекта

Код написан в учебных целях — это урок в курсе по Python и веб-разработке на сайте [Devman](https://dvmn.org).

