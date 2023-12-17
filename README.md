# Adguard_synology_docker_compose Открыть Container Manager, перейти на вкладку Проект, нажать создать проект, создать Docker-compose.yml и вставить содержимое. Обязательно создать структуру папок docker/adguardhome/work, /docker/adguardhome/work/data и /docker/adguardhome/conf и указать путь установки /docker/adguardhome


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
