
# Домашнее задание к занятию «Введение в Terraform»  Губайдуллин Андрей Фаритович

### Задание 1

1. Перейдите в каталог [**src**](https://github.com/netology-code/ter-homeworks/tree/main/01/src). Скачайте все необходимые зависимости, использованные в проекте. 
2. Изучите файл **.gitignore**. В каком terraform-файле, согласно этому .gitignore, допустимо сохранить личную, секретную информацию?(логины,пароли,ключи,токены итд)
3. Выполните код проекта. Найдите  в state-файле секретное содержимое созданного ресурса **random_password**, пришлите в качестве ответа конкретный ключ и его значение.
4. Раскомментируйте блок кода, примерно расположенный на строчках 29–42 файла **main.tf**.
Выполните команду ```terraform validate```. Объясните, в чём заключаются намеренно допущенные ошибки. Исправьте их.
5. Выполните код. В качестве ответа приложите: исправленный фрагмент кода и вывод команды ```docker ps```.
6. Замените имя docker-контейнера в блоке кода на ```hello_world```. Не перепутайте имя контейнера и имя образа. Мы всё ещё продолжаем использовать name = "nginx:latest". Выполните команду ```terraform apply -auto-approve```.
Объясните своими словами, в чём может быть опасность применения ключа  ```-auto-approve```. Догадайтесь или нагуглите зачем может пригодиться данный ключ? В качестве ответа дополнительно приложите вывод команды ```docker ps```.
8. Уничтожьте созданные ресурсы с помощью **terraform**. Убедитесь, что все ресурсы удалены. Приложите содержимое файла **terraform.tfstate**. 
9. Объясните, почему при этом не был удалён docker-образ **nginx:latest**. Ответ **ОБЯЗАТЕЛЬНО НАЙДИТЕ В ПРЕДОСТАВЛЕННОМ КОДЕ**, а затем **ОБЯЗАТЕЛЬНО ПОДКРЕПИТЕ** строчкой из документации [**terraform провайдера docker**](https://library.tf/providers/kreuzwerker/docker/latest).  (ищите в классификаторе resource docker_image )

### Ответ:
1. <img width="813" height="171" alt="image" src="https://github.com/user-attachments/assets/7e9cbd7f-8076-4ae6-9bf6-42859a03112d" />

 <img width="591" height="65" alt="image" src="https://github.com/user-attachments/assets/2fecc9d4-19b1-45e5-a552-38c0ee98a298" />
 
2. <img width="307" height="200" alt="image" src="https://github.com/user-attachments/assets/ffbb97a2-a73d-45e9-b9cd-398f4f902674" />

Согласно файлу .gitignore, для личных и секретных данных  напримерр логинов, паролей, ключей, токенов  предназначен файл personal.auto.tfvars.

3.  <img width="857" height="506" alt="image" src="https://github.com/user-attachments/assets/5940a126-fcd2-4533-a610-29788847b5e2" />
<img width="690" height="528" alt="image" src="https://github.com/user-attachments/assets/ab6fa142-da4a-4b51-adeb-8adb220b453a" />

 сгенерированное секретное значение хранится в ключе result : "hLvmTedz4yyN2Yo8" 


 4. <img width="795" height="265" alt="image" src="https://github.com/user-attachments/assets/9f3dcfcf-fdbe-4494-aeeb-a2d6b78b51cd" />
<img width="757" height="142" alt="image" src="https://github.com/user-attachments/assets/4d7375fe-aab7-4968-9f14-2966e08a4803" />


Ошибка 1: В docker_image не было указано имя ресурса. Исправил на docker_image "nginx" 

Ошибка 2: Имя ресурса docker_container "1nginx" начиналось с цифры. должно начинаться с буквы или подчеркивания. Исправил на docker_container "nginx" 

Ошибка 3: В имени переменной для пароля были опечатки (random_string_FAKE.resulT). Исправил на random_password.random_string.result.


5. <img width="867" height="126" alt="image" src="https://github.com/user-attachments/assets/fcfee1cc-d8bb-488d-a470-85cae99d7733" />

resource "docker_image" "nginx" {
  name         = "nginx:latest"
  keep_locally = true
}

resource "docker_container" "nginx" {
  image = docker_image.nginx.image_id
  name  = "example_${random_password.random_string.result}"

  ports {
    internal = 80
    external = 9090
  }
}


<img width="978" height="625" alt="image" src="https://github.com/user-attachments/assets/2c15552a-dfb2-49ed-a091-a573ecc4e2ea" />


6. <img width="614" height="278" alt="image" src="https://github.com/user-attachments/assets/c607793b-17e8-4b59-85a5-1c426991ecb2" />


<img width="879" height="345" alt="image" src="https://github.com/user-attachments/assets/5ed9905f-db3f-41ce-a486-fe25fde3a9b1" />

Флаг -auto-approve отключает интерактивное подтверждение плана. Terraform немедленно применяет все запланированные изменения, не давая пользователю возможности просмотреть план и убедиться в его безопасности. Это может привести к случайному уничтожению или изменению критичных ресурсов (баз данных, сетей, дисков), если конфигурация содержит ошибку или неожиданное изменение.

для чего нужен -auto-approve:

Ключ предназначен для автоматизированных сценариев, где человек физически не может подтвердить план: в CI/CD-пайплайнах, скриптах развёртывания, тестовых средах и т.п. В таких случаях план обычно предварительно проверяется на этапе terraform plan, а -auto-approve используется только для заранее утверждённых конфигураций.


7. <img width="889" height="422" alt="image" src="https://github.com/user-attachments/assets/f684a8fd-a413-4fbf-b6e6-c40954821fe2" />
<img width="573" height="47" alt="image" src="https://github.com/user-attachments/assets/6b07ca20-59bf-457a-a3cd-32cd7a550f54" />

<img width="705" height="158" alt="image" src="https://github.com/user-attachments/assets/8c0423e8-b03d-403a-a4d0-c1db4973a09e" />


8.<img width="925" height="52" alt="image" src="https://github.com/user-attachments/assets/5b433c04-ee52-4b67-8879-3810ca202cb0" />

при terraform destroy - Terraform не удаляет образ nginx:latest из локального хранилища Docker, потому что это  задано  keep_locally = true. Образ остаётся на хосте и может быть использован повторно

------ 

## Дополнительное задание (со звёздочкой*)

**Настоятельно рекомендуем выполнять все задания со звёздочкой.** Они помогут глубже разобраться в материале.   
Задания со звёздочкой дополнительные, не обязательные к выполнению и никак не повлияют на получение вами зачёта по этому домашнему заданию. 

### Задание 2*

1. Создайте в облаке ВМ. Сделайте это через web-консоль, чтобы не слить по незнанию токен от облака в github(это тема следующей лекции). Если хотите - попробуйте сделать это через terraform, прочитав документацию yandex cloud. Используйте файл ```personal.auto.tfvars``` и гитигнор или иной, безопасный способ передачи токена!
2. Подключитесь к ВМ по ssh и установите стек docker.
3. Найдите в документации docker provider способ настроить подключение terraform на вашей рабочей станции к remote docker context вашей ВМ через ssh.
4. Используя terraform и  remote docker context, скачайте и запустите на вашей ВМ контейнер ```mysql:8``` на порту ```127.0.0.1:3306```, передайте ENV-переменные. Сгенерируйте разные пароли через random_password и передайте их в контейнер, используя интерполяцию из примера с nginx.(```name  = "example_${random_password.random_string.result}"```  , двойные кавычки и фигурные скобки обязательны!) 
```
    environment:
      - "MYSQL_ROOT_PASSWORD=${...}"
      - MYSQL_DATABASE=wordpress
      - MYSQL_USER=wordpress
      - "MYSQL_PASSWORD=${...}"
      - MYSQL_ROOT_HOST="%"
```

6. Зайдите на вашу ВМ , подключитесь к контейнеру и проверьте наличие секретных env-переменных с помощью команды ```env```. Запишите ваш финальный код в репозиторий.
   

### Задание 3*
1. Установите [opentofu](https://opentofu.org/)(fork terraform с лицензией Mozilla Public License, version 2.0) любой версии
2. Попробуйте выполнить тот же код с помощью ```tofu apply```, а не terraform apply.
------

### Правила приёма работы

Домашняя работа оформляется в отдельном GitHub-репозитории в файле README.md.   
Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.

### Критерии оценки

Зачёт ставится, если:

* выполнены все задания,
* ответы даны в развёрнутой форме,
* приложены соответствующие скриншоты и файлы проекта,
* в выполненных заданиях нет противоречий и нарушения логики.

На доработку работу отправят, если:

* задание выполнено частично или не выполнено вообще,
* в логике выполнения заданий есть противоречия и существенные недостатки. 

