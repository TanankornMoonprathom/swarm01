#  wakatime
https://wakatime.com/@spcn01/projects/mrodfccdqw?start=2023-03-05&end=2023-03-11
# Ref awaresome-compose
https://github.com/docker/awesome-compose/tree/master/apache-php
# URL apache-php
https://tan-swarm01.xops.ipv9.me/
# git swarm01
https://github.com/TanankornMoonprathom/swarm01.git


# ขั้นตอนในการทำงาน
# 1. สร้าง Image from Dockerfile
 1. สร้าง index.php
    <details>
    <summary>Show code</summary>

    ```ruby   
    <!DOCTYPE html>
    <html>
    <head>
    <title>Store Login</title>
    <style>
    /* Set the background color to gray */
    h1{
    text-align: center;
    }
    body {
    background-color: blue;
    }
    .topic {          
    text-align: center;
    }
    /* Center the elements on the page */
    .center {
    display: flex;
    justify-content: center;
    align-items: center;
    }
    /* Set the text color to white */
    .white-text {
    color: purple;
    }
    </style>
    </head>
    <body>
    <div class="topic">
    <h1 class="white-text">spcn01 Login</h1>
    </div>
    <div class="center">
    <form>
    <label for="username" class="white-text">Username:</label><br>
    <input type="text" id="username" name="username"><br>
    <label for="password" class="white-text">Password:</label><br>
    <input type="password" id="password" name="password"><br><br>
    <input type="button" value="Login" onclick="login()" class="white-text">
    </form> 
    </div>
    <div class="center">
    <h1><?php
    date_default_timezone_set("Asia/Bangkok");
    echo'Today ';
    echo date('d/m/y');
    echo '<br/>';
    echo'Time ';
    echo date('H:i:s'); 
    echo '<br/>';
    ?></h1>
    </div>
    <script>
    function login() {
    // Get the username and password from the form
    var username = document.getElementById('username').value;
    var password = document.getElementById('password').value;
    // Validate the username and password
    if (username == 'admin' && password == 'password') {
    // If the username and password are correct, redirect the user to the store homepage
    window.location.href = 'store-homepage.html';
    } else {
    // If the username and password are incorrect, display an error message
    alert('Invalid username or password');
    }
    }
    </script>
    </body>
    </html>
    ```

    </details>
 2. สร้าง Dockerfile
    <details>
    <summary>Show code</summary>

    ```ruby
    FROM --platform=$BUILDPLATFORM php:8.0.9-apache as builder #image container

    WORKDIR /var/www/html/ #Set path working command on container

    COPY . /var/www/html/ #Copy file on host to container

    EXPOSE 80 #Set port container allow host access

    CMD ["apache2-foreground"] #run last command before docker create container

    FROM builder as dev-envs

    RUN <<EOF
    apt-get update
    apt-get install -y --no-install-recommends git
    EOF 
    #run command on container

    RUN <<EOF
    useradd -s /bin/bash -m vscode
    groupadd docker
    usermod -aG docker vscode
    EOF

    COPY --from=gloursdocker/docker / /

    CMD ["apache2-foreground"]
    ```

    </details>
 3. Build image from Dockerfile
 
    ```
    docker build . -t <usernameDockerHub>/<repo>:<tag> #tanankorn/apache-php-web:0205
    ```
 4. Push image ไปยัง DockerHub

     ```
     docker push <image ID> <usernameDockerHub>/<repo>:<tag> #tanankorn/apache-php-web:0205
     ```

# 2.สร้าง docker-compose.yml
 1. สร้างไฟล์ docker-compose.yml
    <details>
    <summary>Show code</summary>

    ```ruby
    version: '3.3'
    services:
    web:
    image: tanankorn/apache-php-web:0205
    networks:
    - webproxy
    logging:
    driver: json-file
    volumes:
    - app:/var/www/html/
    container_name: swarm01-web2
    deploy:
    replicas: 1
    labels:
    - traefik.docker.network=webproxy
    - traefik.enable=true
    - traefik.http.routers.${APPNAME}-https.entrypoints=websecure
    - traefik.http.routers.${APPNAME}-https.rule=Host("${APPNAME}.xops.ipv9.me")
    - traefik.http.routers.${APPNAME}-https.tls.certresolver=default
    - traefik.http.services.${APPNAME}.loadbalancer.server.port=80
    resources:
    reservations:
    cpus: '0.1'
    memory: 10M
    limits:
    cpus: '0.4'
    memory: 50M
    networks:
    webproxy:
    external: true
    volumes:
    app:
    ```
# 3.Push docker-compose.yml to github swarm01
# 4.เปิด https://portainer.ipv9.me/
 
 ![image](https://user-images.githubusercontent.com/119097663/224484388-a617001c-cf34-49ce-9d7a-3c3d4b8bfc76.png)

# 5.Click Cluster Xopx.ipv9.xyz on Portainer
 
 ![image](https://user-images.githubusercontent.com/119097663/224484436-f6e5f9a5-5520-409b-8d12-1cfc947404f5.png)
 
# 6.Click menu Stack บน Cluster Xopx.ipv9.xyz
 
 ![image](https://user-images.githubusercontent.com/119097663/224484471-88edcac4-dcd8-437d-b741-ead184381b48.png)

# 7.Click ปุ่ม Add Stack

![image](https://user-images.githubusercontent.com/119097663/224484514-0e6de6f0-c04e-44bf-bddb-df9bf0b2bb83.png)

# 8.Click Build medthod is Repository
 
 ![image](https://user-images.githubusercontent.com/119097663/224484639-134b525a-bae2-4187-92ad-f1ee12e08084.png)

 - Name = name Stack
  - Repository URL = https://github.com/TanankornMoonprathom/swarm01.git
  - Repository reference = refs/heads/main
  - Compose path = name Compose file
  - Automatic updates = enable
    - Fetch interval = time check change on compose file from github 

