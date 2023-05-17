# Дипломный проект.
SkillFactory / Group32
<H1> Спринт 1. </H1>
<H2> Цель: </H2>
Создать Kubernets Cluster <br>
<H2> Задача: </H2> 
Опишите инфраструктуру будущего проекта в виде кода с инструкциями по развертке, нужен кластер Kubernetes и служебный сервер (будем называть его srv). <br>
<H3>Задание 1. Выбираем облачный провайдер и инфраструктуру. </h3>
1. В качествет облачного провайдера был выбран <b> Yandex Cloud</b> <br>
2. В качестве описания серверной инфраструктуры в облаке был выбран <b>Terraform</b> <br>
3. В качестве K8S cluster был выбран <b>Kubespray</b> <br>
4. Cluster K8S будет разворачиваться при помощи <b>Ansible</b> <br>
5. Для достижения данной цели использовал отличную инструкцию: <b><a href="https://git.cloud-team.ru/lections/kubernetes_setup/raw/master/presentation.pdf">Установка кластера Kubernetes</a> и <a href="https://www.youtube.com/watch?v=WFXlr0bVTAQ">Youtube</a> </b><br>
6. Так же для автоматизации будем использовать Bash скрипты.
<br>
<H4> Установка и первоначальная настройка сервера <b> SRV </b> </H4>
<b>1. Устанавливаем в Yandex Cloud сервер SRV </b> <br>
Задача данного сервера: <br>
a) На данном сервере будем хранить весь наш проект и управлять им. <br>
b) Из данного сервера будем разворачивать <b> K8S cluster </b> использовать будем <b> Kubespray </b> разворачивать будем при помощи <b> Ansible </b> и управлять им.<br>
с) На данном сервере будет выполняться <b> CI/CD piplines </b> <br>
d) Master и Worker ноды будем разворачивать в <b> Yandex Cloud</b> при помощи <b>Terraform</b> <br>
e) Мониторинг SRV сервера и K8S кластера.<br>
h) Отправка оповещаний в telegram о доступности нашего приложения  <br> 
<br>
<b>Задание 2. Первоначальная настройка сервера SRV </b>  <br>
a) Устанавливаем все нужные приложения для работы дальнейшей работы: <br>
   - Создаём ssh ключ в моём случаи <b> "~/.ssh/id_rsa.pub" </b> для terraform и ansible 
   - Python3, PIP3, GIT, остальные зависимости.  
   - Устанавливаем Helm и Kubectl
   - Скачиваем Kubespray по адресу: <a href="https://github.com/kubernetes-sigs/kubespray/releases/tag/v2.19.0"> Kubespray </a>
   - Ansible (нужную версию Ansible нужно смотреть в документации выбранной версии Kubespray)
   - Устанавливаем <b> Terraform </b> по инструкции: <a href="https://cloud.yandex.ru/docs/tutorials/infrastructure-management/terraform-quickstart"> Инструкция по установке и настройке Terraform в Ya облаке</a> <br>
b) Создаём папку для нашего проекта <br>
c) Делаем git clone проекта в нашу папку <br>
<code># git clone https://github.com/Suirus777/skillfactory-diplom.git </code><br>
d) Распоковываем из архива наш скачаный Kubespray в корень проекта. <br>
<br>
<b>Задание 3. Начинаем настройку и установку кластера K8S </b><br> 
   a) Настраиваем Terraform. Переходим в папку с манифестом Terraform который разворачивает серверную инфраструктуру для нашего K8S кластера.<br>
<code># cd terraform </code> <br>
   - Переименовываем файл который будет хранит наши данные для подключения к Yandex Cloude: <br>
<code># cp private.auto.tfvars.example private.auto.tfvars </code> <br>
   - Открываем файл private.auto.tfvars и вносим наши данные для авторизации Yandex Cloude <br>
<code>#vim private.auto.tfvars </code> <br>
<table> <tr><td>(Для информации какие данные нужно вносить)<br>
● yc_token – OAuth-токен для доступа к API  <br>
(https://cloud.yandex.ru/docs/iam/concepts/authorization/oauth-token) <br>
● yc_cloud_id – ID облака (скопировать из консоли управления) <br>
● yc_folder_id – ID каталога (скопировать из консоли управления) <br>
</td></tr></table>
   - Terraform манифест называется <b> "k8s-cluster.tf" </b> он имеет код согласно задачи для создания 2 серверов кластера K8S Master и Worker <br>
Но можно изменить настройки и увеличить количество нод, также добавить ingress ноды. Но согласно задачи нужны 2 сервера.<br>
   - Инициализируем Terraform команандой:
<code> terraform init </code> <br>
   - Если всё было правильно настроено то инициализация должна пройти без ошибок.  <br>
b) Подготавливаем настройки для нашего K8S кластера который будет установлен на нодах. Все настройки хранятся в папке <b>/kubespray_inventory</b> <br> 
   - Устанавливаем требуемые приложения <br>
<code> # pip3 install -r kubespray/requirements.txt </code> <br>
с) Запускаем установку K8S кластера: <br>
   - для установки кластера нужно использовать в корне проекта Bash скрипт <b>cluster_install.sh </b><br>
<code>#bash cluster_install.sh </code> <br>
- Данный скрипт инициализирует Terraform на создание 2 серверов Master и Worker <br>
- Иницализирует Ansible playboks для установки K8S кластера на серверах.<br>
- Создаёт конфигурационный файл для управления K8S кластером с сервера SRV <br>
- Копирует конфиг файлы в нужные нам папки для дальнейшего управления K8S кластером с сервера SRV <br>
- Настраивает сервер SRV для работы с K8S кластером  <br>
- При помощи Ansible устанавливает нужны нам приложения на сервере SRV (Docker, Docker-compose, Gitlab-CI) <br>
Результат выполнения скрипта: <br>
<img src="https://github.com/Suirus777/skillfactory-diplom/blob/main/images/K8S_cluster_create.JPG">
- Для удаления кластера нужно использовать bash скрипт: cluster_destroy.sh
<code># bash cluster_destroy.sh </code><br>
Результат выполнения скрипта: <br>
<img src="https://github.com/Suirus777/skillfactory-diplom/blob/main/images/Terraform_remove_cluster.JPG"> 

<H3>4)Кластер установлен и настроен. Готов к деплою нашего приложения. Спринт 1 закончен.</H3>
<h1> Спринт 2 </h1>
<H2> Цель: </H2>
Cобрать и задеплоить приложение из нашего Git в созданный кластер Kubernetes <br>
<H2> Задача: </H2> 
1) Клонируем репозиторий, собираем его на сервере srv.  <br>
2) Описываем приложение в Helm-чарт.  <br>
3) Описываем стадию деплоя в Helm. <br>
<br>
<H3>Задание 1. Клонируем репозиторий, собираем его на сервере srv.</H3>
- Создал в корне проекта "CICD" и клонировал в неё приложение из github.<br> 
- Исправил все ошибки в Docker фалах приложения, настройках приложения и изменил логин и пароль. <br>
- Чувствительные данные такие как логин и пароль и т.д вынес в отдельный файл "/data/app.var" и добавил в gitignor
<img src="https://github.com/Suirus777/skillfactory-diplom/blob/main/images/app.var.JPG">
- После чего создал Docker образ и запустил приложение в Docker. <br>
 
