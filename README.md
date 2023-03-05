#  wakatime
https://wakatime.com/@spcn01/projects/mrodfccdqw?start=2023-02-21&end=2023-02-27

# Ref awaresome-compose
https://github.com/docker/awesome-compose/tree/master/apache-php


# ขั้นตอนการติดตั้ง และใช้งาน ใน VM

 1. Set Template 

    - set time
      ```
      timedatectl set-timezone Asia/Bangkok
      ```

    - install Docker
      ```
      apt update; apt upgrade -y #อัปเดตแพ็คเกจภายในเครื่อง

      apt-get install ca-certificates curl wget gnupg lsb-release -y #ติดตั้งแพ็คเกจ

      mkdir -m 0755 -p /etv/apt/keyrings

      curl -fsSL https://download.docker.com/linux/ubuntu/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg #ดาวโหลดไฟล์แพ็คเกจ Docker

      echo \ "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \ $(lsb_release -cs) stable" |  tee /etc/apt/sources.list.d/docker.list > /dev/null

      apt-get update #อัปเดทไฟล์แพ็คเกจเพื่อไว้สำหรับให้ติดตั้ง
      apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y #ติดตั้ง Docker

      reboot
      ```

 2. Clone Templete ออกมา 3 Node คือ
    - manage
    - work1
    - work2

 3. Set Hostname
    ```
    hostnamectl set-hostname "ชื่อ Hostname โดยต้องห้ามซ้ำ" #spcn19-swarm01
    ```

 4. Reset Machine ID เพื่อขอ Public IP จาก DHCP 
    ```
    cp /dev/null /etc/machine-id
    rm /var/lib/dbus/machine-id
    ln -s /etc/machine-id /var/lib/dbus/machine-id
    init 0
    ```
 5. ทำการเตรียม stack swarm and Portainer CE 
    ```
# Stack Swarm
<a name="stack-swarm"></a>

 - Manager Swarm

   - Swarm init
     ```
     docker swarm init #รันในเครื่อง Manage
     ```

   - นำ Token Url ไป run บน worker ทุก Node ที่ต้องการให้เชื่อมต่อ

   - Check Node Stack swarm
     ```
     docker node ls
     ```

   - install portainer CE
     ```
     curl -L https://downloads.portainer.io/ce2-17/portainer-agent-stack.yml -o portainer-agent-stack.yml
     docker stack deploy -c portainer-agent-stack.yml portainer
     ```

   ### Ref
   - https://github.com/pitimon/dockerswarm-inhoure#swarm-init