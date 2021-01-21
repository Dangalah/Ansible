# Ansible

Запуск вагранта 
vagrant destroy -f && vagrant up

запуск ансибл
ansible-playbook ansible/play.yml -kK

запуск curl
curl http://10.211.0.10/service_data

ВАЖНО!!!!

Меняем hosts и переносим в etc/ansible

Для работы ansible необходимо подключиться к машине и записать ключ 
ssh vagrant@10.211.0.10

Login: vagrant
password: vagrant
