#  wakatime
https://wakatime.com/@spcn01/projects/mrodfccdqw?start=2023-03-05&end=2023-03-11
# Ref awaresome-compose
https://github.com/docker/awesome-compose/tree/master/apache-php
# URL apache-php
https://tan-swarm01.xops.ipv9.me/


# ขั้นตอนในการทำงาน
1. Create Image from Dockerfile
# Create Image from Dockerfile
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
    # syntax=docker/dockerfile:1.4

FROM --platform=$BUILDPLATFORM php:8.0.9-apache as builder

WORKDIR /var/www/html/

COPY . .

EXPOSE 80

CMD ["apache2-foreground"]

FROM builder as dev-envs

RUN <<EOF
apt-get update
apt-get install -y --no-install-recommends git
EOF

RUN <<EOF
useradd -s /bin/bash -m vscode
groupadd docker
usermod -aG docker vscode
EOF
# install Docker tools (cli, buildx, compose)
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