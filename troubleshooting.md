# Fundamentos y usos prácticos de Docker

Documento para Troubleshooting

### Troubleshooting (Opcional)

> [!NOTE]  
> Si no pudo conectar la aplicación a la base de datos y no puede encontrar la falla, dejamos a continuación algunas sugerencias que le pueden ayudar.

#### Verificar base de datos

- Una vez arrancado el contenedor de base de datos si el contenedor está arriba verifique que la base de datos esté arriba con el siguiente comando

    ```bash
    docker exec -it <mysql-container-id> mysql -u root -p
    ```

- Una vez que escriba la password entrará a la shell de mysql. Verificamos que exista la base de datos `todos`.

    ```mysql
    mysql> SHOW DATABASES;
    ```

- Deberías ver algo así

    ```bash
    +--------------------+
    | Database           |
    +--------------------+
    | information_schema |
    | mysql              |
    | performance_schema |
    | sys                |
    | todos              |
    +--------------------+
    5 rows in set (0.00 sec)
    ```

- Para salir de la shell de MySQL escriba:

    ```bash
    mysql> exit
    ```

#### Verificar conectividad

Una vez que hayamos comprobado que la base de datos está corriendo y la base de datos exista, verificaremos la conectividad hacia el contenedor donde corre MySQL. Para esto, usaremos la imágen [`nicolaka/netshot`](https://github.com/nicolaka/netshoot) que viene con muchas herramientas útiles para solucionar o depurar problemas de red.

1. Inicie un nuevo contenedor utilizando la imaǵen `nicolaka/netshoot`. Asegurese de conectarse a la misma red. Suponiendo que la red elegida fue `todo-app`, el comando es:

    ```bash
    docker run -it --network todo-app nicolaka/netshoot
    ```

2. Una vez dentro del contenedor utilizaremos el comando `dig`, que es una herramienta de DNS. Si el nombre que eligió para la base de datos, mediante el parámetro `--name` o `--network-alias` fue `db` el comando sería el siguiente:

    ```bash
    dig db
    ```

- Debería tener un resultado como el siguiente:

    ```
    ; <<>> DiG 9.18.25 <<>> db
    ;; global options: +cmd
    ;; Got answer:
    ;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 58769
    ;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 0

    ;; QUESTION SECTION:
    ;db.				IN	A

    ;; ANSWER SECTION:
    db.			600	IN	A	172.31.0.2

    ;; Query time: 0 msec
    ;; SERVER: 127.0.0.11#53(127.0.0.11) (UDP)
    ;; WHEN: Mon Sep 16 02:02:48 UTC 2024
    ;; MSG SIZE  rcvd: 38
    ```

- Observe la **ANSWER SECTION**. Si todo está bien, verá un registro `A` que se resuelve a una dirección IP, en este caso la `172.31.0.2`. En su prueba probablemente tendrá un valor diferente.
- Si no aparece la sección ANSWER es que no se puede resolver la dirección IP del nombre. Por lo tanto debe verificar que estén conectado a la misma red y que el nombre del host esté bien escrito.


--------------


<p align="center">
  <img src="./imgs/logos.footer.gray.webp">
</p>