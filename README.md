# connect ssh to NAS:
# ifconfig (для проверки eth0 или ovs_eth0) 
# sudo docker network create -d macvlan -o parent=ovs_eth0 --subnet=192.168.1.0/24 --gateway=192.168.1.1 --ip-range=192.168.1.200/32 adguard_network (выбрать свою подсеть, не повторяющийся ip  и родительский интерфейс ovs_eth0 или eth0)
# Adguard_synology_docker_compose Открыть Container Manager, перейти на вкладку Проект, нажать создать проект, создать Docker-compose.yml и вставить содержимое. Обязательно создать структуру папок docker/adguardhome/work, /docker/adguardhome/work/data и /docker/adguardhome/conf и указать путь установки /docker/adguardhome
# После установки в Container Manager перейти в Сеть и объеденить две сети Bridge и adguard_network

version: "1.1"
services:
  adguardhome_macvlan:
    image: adguard/adguardhome:latest   
                                        
    container_name: adguardhome
    
    hostname: AdGuard-Home      
    
    environment:
           
      - TZ=Europe/Moscow
      - LANG=ru_RU.UTF8
      - LANGUAGE=ru_RU.UTF8

    volumes:
      - "/volume1/docker/adguardhome/work:/opt/adguardhome/work"
      - "/volume1/docker/adguardhome/conf:/opt/adguardhome/conf"

    ports:
      -  "353:53"
      -  "367:67/udp"
      -  "368:68"
      -  "3080:80/tcp"
      -  "3443:443/tcp"
      -  "3853:853/tcp"
      -  "3030:3000/tcp"

    restart: unless-stopped

    healthcheck:
      test: "/bin/netstat -pant | /bin/grep 53"
      interval: 45s
      timeout: 30s
      retries: 3

networks:
  macvlan-network:        
    external: true
