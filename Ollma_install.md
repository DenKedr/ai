Установка Ollma на Ubuntu 24.04.01

1. Создаем скрипт по установке
<code>nano install_ollama.sh</code>

1.1 Далее вставляем код
<code>
#!/bin/bash

# Обновление пакетов
sudo apt update && sudo apt upgrade -y

# Установка зависимостей
sudo apt install -y ca-certificates curl gnupg

# Добавление Docker в систему
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo tee /etc/apt/keyrings/docker.asc > /dev/null
sudo chmod a+r /etc/apt/keyrings/docker.asc
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update

# Установка Docker
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Добавление текущего пользователя в группу Docker
sudo usermod -aG docker $USER
echo "Для применения изменений, выйдите из системы и войдите снова, или перезагрузите сервер."

# Установка Ollama
curl -fsSL https://ollama.com/install.sh | sh

# Проверка версии Ollama
ollama --version

# Запуск OpenWebUI через Docker
docker run -d -p 3000:8080 \
  --add-host=host.docker.internal:host-gateway \
  -v ollama:/root/.ollama \
  -v open-webui:/app/backend/data \
  --name open-webui \
  --restart always \
  ghcr.io/open-webui/open-webui:ollama

# Перезапуск контейнера OpenWebUI на случай, если он не запустился
docker restart open-webui

# Проверка состояния контейнера OpenWebUI
docker ps

# Уведомление пользователя
echo "Запуск завершён. Откройте браузер и перейдите по адресу: http://0.0.0.0:3000/"
</code>
<br/>)
1.2 Далее Созраняем код<br/>)
1.3 Сделайте файл исполнимым:<br/>)
<code>chmod +x install_ollama.sh</code>
<br/>)
1.3 Запускаем скрипт<br/>)
<code>./install_ollama.sh</code>



