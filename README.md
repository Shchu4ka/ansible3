# Домашнее задание к занятию 2 «Работа с Playbook»

# Домашнее задание к занятию 2 «Работа с Playbook»

1. ![image](https://github.com/Shchu4ka/ansible2/assets/29621873/7e67be6f-8689-489b-ba5a-94470ccb2fe0)
2. Я использую машину на Ubuntu, поэтому переписал установку Clickhouse под ubuntu. Также, был написан плей для установки Vector версии 0.38.0 под Ubuntu
3. При написании плея использовал следующие модули:
- ansible.builtin.get_url - получение адреса
- ansible.builtin.apt - менеджер пакетов apt
- ansible.builtin.meta - модуль для работы с метаданными
- ansible.builtin.pause - пауза 
- ansible.builtin.command - выполнение команды на удаленной машине
- ansible.builtin.file - работа с файлами
- ansible.builtin.unarchive - разархивация
- ansible.builtin.copy - копирование файлов
- ansible.builtin.replace - замена символов
- ansible.builtin.user - модуль работы с пользователями
- ansible.builtin.service - модуль работы с сервисами
- ansible.builtin.systemd - модуль работы с системными демонами
4. ![image](https://github.com/Shchu4ka/ansible2/assets/29621873/24e1e6b4-543a-43ff-b240-1ee54e5c9884)
5. ![image](https://github.com/Shchu4ka/ansible2/assets/29621873/9700e604-dafc-42ec-bc04-54b75622946f)
6. Выполнение останавливается так как не удовлетворена зависимость (файл физически не скачан и на хосте отсутствует)
  ![image](https://github.com/Shchu4ka/ansible2/assets/29621873/6258a6da-9753-439e-8e15-eca546043877)
7. ![image](https://github.com/Shchu4ka/ansible2/assets/29621873/6db56424-cf74-4db1-9768-4a206a73ba19)
![image](https://github.com/Shchu4ka/ansible2/assets/29621873/795a74bf-3c0c-434b-9069-3ced6461c6f1)
![image](https://github.com/Shchu4ka/ansible2/assets/29621873/bf2f1d9c-966b-4240-afc5-34b4afc22efa)
8. Изменения в моем случае будут, так как я создаю и удаляю рабочую директорию для Vector, а также, меняю конфиг-файлы
![image](https://github.com/Shchu4ka/ansible2/assets/29621873/525a3855-b5a1-4f80-857b-9601ed01b434)
![image](https://github.com/Shchu4ka/ansible2/assets/29621873/4ccf9407-b92f-4ae2-8495-f296ffef69c4)
![image](https://github.com/Shchu4ka/ansible2/assets/29621873/55c327f8-c49b-4f0d-8c08-1184814b6afe)
9. Плейбук содержит 2 блока тасок:

Первый блок устанавливает Clickhouse, ему присвоен тег clickhouse
В блоке используются переменные:
clickhouse_version - версия Clickhouse
clickhouse_packages - список пакетов для установки
workdir - рабочая директория для скачивания пакетов
Задачи:
- Clickhouse | Get distrib - скачивает пакеты из репозитория
- Clickhouse | Install package clickhouse-common-static - установка основного пакета Clickhouse-common
- Clickhouse | Install package clickhouse-client - установка пакета клиента Кликхауса
- Clickhouse | Install package clickhouse-server - установка пакета сервера Кликхауса
- Clickhouse | Flush handlers - запуск хэндлера для старта сервиса Кликхауса
- Clickhouse | Waiting for start - пауза в 10 секунд для старта сервера, без него есть проблемы с созданием БД, так как сервер не успевает стартовать
- Clickhouse | Create database - создание БД Кликхауса

Второй блок устанавливает Vector, ему присвоен тэг vector.
В блоке используются параметры:
vector_version - версия Vector
workdir - рабочая директория для скачивания пакетов

Задачи:
- Vector | Create work directory - создаем поддиректорию для скачанных пакетов и удобства последующей очистки
- Vector | Get distributive - скачиваем архив с дистрибутивом
- Vector | Unzip - распаковываем архив
- Vector | Install binary - копируем бинарник Vector в /usr/bin
- Vector | Check installation - выполняем команду vector --version для проверки корректности работы бинарника
- Vector | Create Vector config - копируем шаблонный конфиг в /etc/vector
- Vector | Create vector.service daemon - создаем демон для работы вектора
- Vector | Modify vector.service file - редактируем демона, чтобы он стартовал с нужным конфигом
- Vector | Create user vector - создаем пользователя vector
- Vector | Create data_dir - создаем папку для хранения данных Vector
- Vector | Remove work directory - удаляем рабочую папку с распакованным архивом
