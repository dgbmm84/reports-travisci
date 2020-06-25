---
Depliegue usando Travis/fabric usando un docker-compose PROD
---

Travis configuration
    
    - En la raíz del proyecto existirá un fichero .travis.yml
    - Travis funciona con repositorios ṕublicos compartidos. Por ello hay un repo en GitHub con este proyecto.
        - Si se le asocia directamente un fichero .travis.yml en vez de .github/workflows desplegará travis
    - Configuración del repositorio travis:
        -- -- TODO -- 

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
        - Las variables de la aplicación están declaradas en el apartado --- TODO --- y se injectan automáticatimante.  
            - Fabric usa para task arguments un tratamiento especial en los caracteres que hay que aplicar a las variables de entorno 
                - En este caso para ENV_FILE_PROD (https://docs.fabfile.org/en/1.8/usage/fab.html#per-task-arguments)

Comandos

    - docker ps
        Mostrará tres servicios levantados (app / nginx / mysql)
        Para para los contenedores y eliminar volumenes docker-compose -f docker-compose-prod.yml down (-v)
        