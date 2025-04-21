Исправленная инструкция по установке Ollama на Ubuntu 24.04.01 <br/> <br/>
1. Обновление пакетов <br/>
sudo apt update && sudo apt upgrade -y <br/>
Эта команда обновляет информацию о пакетах и устанавливает доступные обновления. <br/>
 <br/> <br/>
2. Установка зависимостей <br/>
sudo apt install -y ca-certificates curl gnupg lsb-release <br/>
Эта команда установит основные зависимости, включая утилиты для работы с репозиториями и сертификатами. <br/>
 <br/> <br/>
3. Добавление Docker в систему <br/>
Добавление GPG ключа Docker: <br/>
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo tee /etc/apt/keyrings/docker.asc > /dev/null <br/>
Установка репозитория Docker: <br/>
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null <br/>
Обновление пакетов: <br/>
sudo apt update <br/>
 <br/> <br/>
4. Установка Docker <br/>
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin <br/>
Эта команда установит Docker и все необходимые компоненты. <br/>
 <br/> <br/>
5. Добавление текущего пользователя в группу Docker <br/>
sudo usermod -aG docker $USER <br/>
Примечание: После этой команды необходимо выйти из системы и войти снова или перезагрузить сервер, чтобы изменения вступили в силу. <br/>
 <br/> <br/>
6. Установка Ollama <br/>
curl -fsSL https://ollama.com/install.sh | sh <br/>
Эта команда скачает и установит Ollama на сервер. <br/>
 <br/> <br/>
7. Проверка версии Ollama <br/>
ollama --version <br/>
Команда позволяет убедиться, что Ollama установлен корректно. <br/>
 <br/> <br/>
8. Открытие нужных портов <br/>
Перед запуском контейнера OpenWebUI, откройте порты, если это необходимо: <br/>
sudo ufw allow 3000/tcp <br/>
Примечание: Если используется ufw (Uncomplicated Firewall), вам нужно будет открыть порт 3000 для доступа к веб-интерфейсу. <br/>
 <br/> <br/>
9. Запуск OpenWebUI через Docker <br/>
docker run -d -p 3000:8080 \
  --add-host=host.docker.internal:host-gateway \
  -v ollama:/root/.ollama \
  -v open-webui:/app/backend/data \
  --name open-webui \
  --restart always \
  ghcr.io/open-webui/open-webui:ollama <br/>
Эта команда запускает контейнер Docker с OpenWebUI, монтируя необходимые тома и указывая параметры для перезапуска контейнера в случае сбоев.
 <br/> <br/>
10. Перезапуск контейнера OpenWebUI на случай, если он не запустился <br/>
docker restart open-webui <br/>
Команда для перезапуска контейнера, если он не запустился или был остановлен. <br/>
 <br/> <br/>
11. Проверка состояния контейнера OpenWebUI <br/>
docker ps <br/>
Эта команда покажет список всех работающих контейнеров и их состояние. <br/>
 <br/> <br/>
12. Уведомление пользователя <br/>
echo "Запуск завершён. Откройте браузер и перейдите по адресу: http://0.0.0.0:3000/" <br/>
Это сообщение для пользователя, информирующее о том, что контейнер работает и интерфейс доступен по адресу http://0.0.0.0:3000/. <br/>
 <br/> <br/> <br/>

Проверка автозапуска контейнера <br/>
Чтобы убедиться, что контейнер настроен правильно и будет перезапущен после перезагрузки, вы можете проверить статус контейнера командой: <br/>
docker ps -a <br/>
 <br/> <br/>
Дополнительные шаги <br/>
Если вы хотите, чтобы система автоматически запускала Docker при старте (если это еще не настроено), вы можете включить автозапуск Docker через систему systemd: <br/>
sudo systemctl enable docker <br/>
