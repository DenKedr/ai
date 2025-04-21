Исправленная инструкция по установке Ollama на Ubuntu 24.04.01
1. Обновление пакетов
sudo apt update && sudo apt upgrade -y
Эта команда обновляет информацию о пакетах и устанавливает доступные обновления.

2. Установка зависимостей
sudo apt install -y ca-certificates curl gnupg lsb-release
Эта команда установит основные зависимости, включая утилиты для работы с репозиториями и сертификатами.

3. Добавление Docker в систему
Добавление GPG ключа Docker:
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo tee /etc/apt/keyrings/docker.asc > /dev/null
Установка репозитория Docker:
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
Обновление пакетов:
sudo apt update

4. Установка Docker
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
Эта команда установит Docker и все необходимые компоненты.

5. Добавление текущего пользователя в группу Docker
sudo usermod -aG docker $USER
Примечание: После этой команды необходимо выйти из системы и войти снова или перезагрузить сервер, чтобы изменения вступили в силу.

6. Установка Ollama
curl -fsSL https://ollama.com/install.sh | sh
Эта команда скачает и установит Ollama на сервер.

7. Проверка версии Ollama
ollama --version
Команда позволяет убедиться, что Ollama установлен корректно.

8. Открытие нужных портов
Перед запуском контейнера OpenWebUI, откройте порты, если это необходимо:
sudo ufw allow 3000/tcp
Примечание: Если используется ufw (Uncomplicated Firewall), вам нужно будет открыть порт 3000 для доступа к веб-интерфейсу.

9. Запуск OpenWebUI через Docker
docker run -d -p 3000:8080 \
  --add-host=host.docker.internal:host-gateway \
  -v ollama:/root/.ollama \
  -v open-webui:/app/backend/data \
  --name open-webui \
  --restart always \
  ghcr.io/open-webui/open-webui:ollama
Эта команда запускает контейнер Docker с OpenWebUI, монтируя необходимые тома и указывая параметры для перезапуска контейнера в случае сбоев.

10. Перезапуск контейнера OpenWebUI на случай, если он не запустился
docker restart open-webui
Команда для перезапуска контейнера, если он не запустился или был остановлен.

11. Проверка состояния контейнера OpenWebUI
docker ps
Эта команда покажет список всех работающих контейнеров и их состояние.

12. Уведомление пользователя
echo "Запуск завершён. Откройте браузер и перейдите по адресу: http://0.0.0.0:3000/"
Это сообщение для пользователя, информирующее о том, что контейнер работает и интерфейс доступен по адресу http://0.0.0.0:3000/.


Проверка автозапуска контейнера
Чтобы убедиться, что контейнер настроен правильно и будет перезапущен после перезагрузки, вы можете проверить статус контейнера командой:
docker ps -a

Дополнительные шаги
Если вы хотите, чтобы система автоматически запускала Docker при старте (если это еще не настроено), вы можете включить автозапуск Docker через систему systemd:
sudo systemctl enable docker
