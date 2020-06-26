---
Depliegue usando Travis/fabric usando un docker-compose PROD
---
https://docs.travis-ci.com/user/tutorial/#to-get-started-with-travis-ci-using-github

Travis configuration
    
    -- SSH Configuration
   
        1 - Install the Travis cli
            sudo apt install ruby ruby-dev
            sudo gem install travis
        2.- Gererar clave id_rsa para Travis bajo el root del proyecto
            ssh-keygen -t rsa -b 4096 -C 'build@travis-ci.org' -f ./deploy_rsa    
        3.- Authenticate with your Github credentials
            travis login --org
        4.- Indicar a travis la clave privada generada para que lo asocie al fichero .travis.yml
            travis encrypt-file deploy_rsa --add
            (The above command creates the encrypted key file: deploy_rsa.enc and adds the decrypt key as and environment variable to the .travis.yml.
             Commit the deploy_rsa.enc to the repository, and delete the unencrypted private key:)
            Este comando añade info openssl al fichero .travis.yml
        5.- rm deploy_rsa
        6.- deploy_rsa.pub to the .ssh/authorized_keys file
        7.- rm deploy_rsa.pub

    
    - En la raíz del proyecto existirá un fichero .travis.yml
    - Travis funciona con repositorios ṕublicos compartidos. Por ello hay un repo en GitHub con este proyecto.
        - Si se le asocia directamente un fichero .travis.yml en vez de .github/workflows desplegará travis
    - Configuración del repositorio travis:
        -- env vars dentro de /reports-travisci/settings 

Host de despliegue 

    - Es local haciendo uso de ngrok para configurar un tunnel por tcp dad una ip pública:
    - No hay necesidad de configurar una VM o VPS
    - Ngrok
        - ngrok tcp 22
        - Actualizar fabfile con el puerto otorgado por ngrok
        
Configuración del host destino

    - Instalación de Docker / Docker-Compose
    - Ruta de despliegue configurada en fabfile.py
        - En dicha ruta haber realizado previamente "git init" para inicialiar repo
        - Configuración del .gti/config
        
Despliegue

    - Git push origin master 
    - Arranca los servicios de docker en la máquina destino
        - docker exec -it app_prod bash
        - Las variables de la aplicación están declaradas en el apartado /reports-travisci/settings se actualizan las vars de los entornos y la SSH_KEY  y se injectan automáticatimante.  
            - Fabric usa para task arguments un tratamiento especial en los caracteres que hay que aplicar a las variables de entorno 
                - En este caso para ENV_FILE_PROD (https://docs.fabfile.org/en/1.8/usage/fab.html#per-task-arguments)

Comandos

    - docker ps
        Mostrará tres servicios levantados (app / nginx / mysql)
        Para para los contenedores y eliminar volumenes docker-compose -f docker-compose-prod.yml down (-v)
        