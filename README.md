#  wakatime
https://wakatime.com/@spcn01/projects/mrodfccdqw?start=2023-03-05&end=2023-03-11
# Ref awaresome-compose
https://github.com/docker/awesome-compose/tree/master/apache-php
# URL apache-php
https://tan-swarm01.xops.ipv9.me/


# ขั้นตอนในการทำงาน
# 1. Create Image from Dockerfile
 1. Create index.php
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
 2. Create Dockerfile
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
 4. Push image to DockerHub

     ```
     docker push <image ID> <usernameDockerHub>/<repo>:<tag> #tanankorn/apache-php-web:0205
     ```

# 2.Create docker-compose.yml
 1. Create docker-compose.yml
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
# 4.Open https://portainer.ipv9.me/
 
 ![image](https://user-images.githubusercontent.com/119097663/224484388-a617001c-cf34-49ce-9d7a-3c3d4b8bfc76.png)

# 5.Click Cluster Xopx.ipv9.xyz on Portainer
# 6.Click menu Stack on Cluster Xopx.ipv9.xyz

<div align="center"><img src="app/image/cluster.png" width="500px"></div>

# 7.Click button Add Stack

<div align="center"><img src="app/image/menuservice.png" width="500px"></div>

# 8.Click Build medthod is Repository

<div align="center"><img src="app/image/addStack.png" width="500px"></div>
