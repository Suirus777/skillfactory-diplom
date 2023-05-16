# SkillFactory Дипломный проект Group32

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
<br>
<H4> Установка и первоначальная настройка сервера <b> SRV </b> </H4>
1. Устанавливаем в Yandex Cloud сервер <b> SRV </b> <br>
Задача данного сервера: <br>
a) На данном сервере будем хранить весь наш проект и управлять им. <br>
b) Из данного сервера будем разворачивать <b> K8S cluster </b> использовать будем <b> Kubespray </b> разворачивать будем при помощи <b> Ansible </b> и управлять им.<br>
с) На данном сервере будет выполняться <b> CI/CD piplines </b> <br>
d) Master и Worker ноды будем разворачивать в <b> Yandex Cloud</b> при помощи <b>Terraform</b> <br>
e) Мониторинг K8S кластера.<br>
<br>
2. Первоначальная настройка сервера <b> SRV </b>  <br>
a) Устанавливаем все нужные приложения для работы дальнейшей работы: <br>
   - Python3, PIP3, GIT, остальные зависимости.  
   - Скачиваем Kubespray по адресу: <a href="https://github.com/kubernetes-sigs/kubespray/releases/tag/v2.19.0"> Kubespray </a>
   - Ansible (нужную версию Ansible нужно смотреть в документации выбранной версии Kubespray)
   - Устанавливаем <b> Terraform </b> по инструкции: <a href="https://cloud.yandex.ru/docs/tutorials/infrastructure-management/terraform-quickstart"> Инструкция по установке и настройке Terraform в Ya облаке</a> <br>
b) Создаём папку для нашего проекта <br>
c) Делаем git clone проекта в нашу папку <br>
<code># git clone https://github.com/Suirus777/skillfactory-diplom.git <br>
d) Распоковываем из архива наш скачаный Kubespray в корень проекта. <br>
<br>


3. Начинаем настройку и установку кластера K8S <br> 



2. Устанавливаем нужные нам инструменты для дальнейшего создани
