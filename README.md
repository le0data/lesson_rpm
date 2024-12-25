# Размещаю свой RPM в своем репозитории

Для работы создавалась ВМ с помощью Vagrant. Box = "bento/almalinux-9.3" , box_version = "202404.23.0"

## Создание своего RPM пакета (На Nginx с дополнительным модулем ngx_brotli)

1) Установка необходимых пакетов

![1_install_packets](https://github.com/user-attachments/assets/0e4508e6-3094-4a46-a7ee-e9763c19e257)

2) Загрузка SRPM пакета Nginx для дальнейшей работы с ним

![2_end_installation_and_start_nginx](https://github.com/user-attachments/assets/725733b8-98a2-47b9-b0bc-b115dd1af75f)

3) Установка зависимостей для сборки пакета Nginx

![3_zavisimosti](https://github.com/user-attachments/assets/d5bde96d-3875-4300-b53c-f4844ae06230)

4) Скачивание исходного кода модуля ngx_brotli

![4_brotli_start](https://github.com/user-attachments/assets/f413d9bc-bd5b-4bf1-9ea9-dd5637f727c7)

5) Сборка модуля ngx_brotli

![5_cmake_brotli](https://github.com/user-attachments/assets/3bf97d8f-2f80-4041-bcfa-763a3d1d90b1)

   Нужно еще поправить сам spec файл, чтобы Nginx собирался с необходимыми опциями: находим секцию с параметрами configure (до условий if) и добавляем указание на модуль (*не забыть указать завершающий обратный слэш*):
   
`--add-module=/root/ngx_brotli \`


6) Сборка RPM пакета

![6_sborka_rpm](https://github.com/user-attachments/assets/cd827da2-5898-4567-8720-4d97a23a6807)

7) Убеждаемся, что все пакеты создались

![7_result_packages](https://github.com/user-attachments/assets/a7397ce5-6c20-4944-a103-fc8a492060e1)

8) Копирую пакеты в общий каталог и устанавливаю пакет 

![8_install_package](https://github.com/user-attachments/assets/cfce30bb-9253-470e-b198-ebb0e41da3b6)

9) Результат

![9_rpm_status_nginx](https://github.com/user-attachments/assets/06fb25ec-b6f5-4227-9dce-096f2e00391d)

## Создание своего репозитория и размещение в нем ранее собранного RPM

1) Создаю свой репозиторий

![10_Create_repository](https://github.com/user-attachments/assets/d57fb07b-8c42-4a67-9048-2bcd08a6b5fd)

2) Для прозрачности настроил в Nginx доступ к листингу каталога. В файле /etc/nginx/nginx.conf в блоке server добавил следующие директивы:

```
  index index.html index.htm;
	autoindex on;
```
![11_listing_catalog_nginx](https://github.com/user-attachments/assets/888295ec-de6f-4208-beb8-73527e4c212c)

3) Прверка синтаксиса и перезапуск Nginx

![12_check_syntax](https://github.com/user-attachments/assets/0fb14024-a529-4542-9275-522315af9968)

4) Просмотр с помощью curl

![13_curl_result](https://github.com/user-attachments/assets/9ec3bb0c-440d-4fa7-aa22-0625b94d8a13)

5) Добавление репозитория в /etc/yum.repos.d

![14_add_to_repository](https://github.com/user-attachments/assets/8b7ec141-e062-4d7a-b91b-98bdb3c01f86)

6) Убеждаемся, что репозиторий подключился и посмотрим, что в нем есть

![15_repository status](https://github.com/user-attachments/assets/13755ca2-8faa-4945-83c6-30ce7cc3f998)

7) Добавляю пакет в репозиторий

![16_add_packege_to_repository](https://github.com/user-attachments/assets/442fc2b1-2d66-4083-abbc-60f1be45e44d)

8) Обновляю список пакетов в репозитории

![17_update_list_in_repository](https://github.com/user-attachments/assets/520aae04-e4cd-425e-8a24-a04a88783dc8)

9) Так как Nginx уже установлен, устанавливаю репозиторий persona-release.

![18_install_repo_start](https://github.com/user-attachments/assets/a0f576b7-76b4-4ecb-b0c9-bca75d831193)

Все готово!

![19_install_repo_finish](https://github.com/user-attachments/assets/dcf2aa97-be75-42df-a01e-436d98c6338a)







