 connect ssh to NAS:
# ifconfig (для проверки eth0 или ovs_eth0) 
# sudo docker network create -d macvlan -o parent=ovs_eth0 --subnet=192.168.1.0/24 --gateway=192.168.1.1 --ip-range=192.168.1.200/32 adguard_network (выбрать свою подсеть, не повторяющийся ip  и родительский интерфейс ovs_eth0 или eth0)
# Adguard_synology_docker_compose Открыть Container Manager, перейти на вкладку Проект, нажать создать проект, создать Docker-compose.yml и вставить содержимое (или импортировать файл adguard-docker-compose.yml). Обязательно создать структуру папок docker/adguardhome/work, /docker/adguardhome/work/data и /docker/adguardhome/conf и указать путь установки /docker/adguardhome
# После установки в Container Manager перейти в Сеть и объеденить две сети Bridge и adguard_network (выбрать сеть - управление - поставить галочку)


