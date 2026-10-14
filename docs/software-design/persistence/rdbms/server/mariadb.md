# Maria DB

![MariaDB Foundation logo](https://d1q6f0aelx0por.cloudfront.net/product-logos/library-mariadb-logo.png){align=right loading=lazy style="height:150px;width:150px"}

MariaDB is a community-developed, commercially supported fork of the MySQL relational database management system, intended to remain free and open-source software under the GNU General Public License.

Development is led by some of the original developers of MySQL, who forked it due to concerns over its acquisition by Oracle Corporation in 2009.

[MariaDB Foundation](https://mariadb.org/){target=_blank .md-button}

[MariaDB - Wikipedia](https://en.wikipedia.org/wiki/MariaDB){target=_blank .md-button}

[MariaDB Primer](https://mariadb.com/kb/en/a-mariadb-primer/){target=_blank .md-button}

## Docker

!!! EXAMPLE "Docker Compose for MariaDB"
    ```Docker title="compose.yaml"

    services:
      db:
        image: mariadb
        restart: always
        environment:
          # MARIADB_RANDOM_ROOT_PASSWORD: yes
          MARIADB_ROOT_PASSWORD: practicalli
          MARIADB_MYSQL_LOCALHOST_USER: clojure
          MARIADB_DATABASE: service-db
        healthcheck:
          test: [ "CMD", "healtcheck.sh --su-mysql --connect --innodb_initialized" ]
          timeout: 45s
          interval: 10s
          retries: 10

      adminer:
        image: adminer
        restart: always
        ports:
          - 8080:8080
    ```

[MariaDB Official Docker Image](https://hub.docker.com/_/mariadb){target=_blank .md-button}

[Install & Use MariaDB via Docker](https://mariadb.com/kb/en/installing-and-using-mariadb-via-docker/){target=_blank .md-button}

[MariaDB healthcheck script](https://mariadb.com/kb/en/using-healthcheck-sh-script/){target=_blank .md-button}
[MariaDB healthcheck script - source](https://github.com/MariaDB/mariadb-docker/blob/master/10.10/healthcheck.sh){target=_blank .md-button}
