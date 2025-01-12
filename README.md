{
    "_comment": "DO NOT EDIT: FILE GENERATED AUTOMATICALLY BY PTERODACTYL PANEL - PTERODACTYL.IO",
    "meta": {
        "version": "PTDL_v2",
        "update_url": null
    },
    "exported_at": "2025-01-10T01:59:44-05:00",
    "name": "\ud83c\udf84 Sylphiette | The Best \ud83c\udf31",
    "author": "katashifukushima23@gmail.com",
    "description": "un egg gen\u00e9rico para node.js\r\n\r\nEsto clonar\u00e1 un repositorio git. Por defecto, usa master si no se especifica una rama.\r\n\r\nInstala los node_modules al instalar. Si estableces user_upload, asumo que sabes lo que est\u00e1s haciendo.",
    "features": null,
    "docker_images": {
        "ghcr.io\/parkervcp\/yolks:nodejs_20": "ghcr.io\/parkervcp\/yolks:nodejs_20"
    },
    "file_denylist": [],
    "startup": "if [[ -d .git ]] && [[ 1 == \"1\" ]]; then git pull; fi; if [[ ! -z ${NODE_PACKAGES} ]]; then \/usr\/local\/bin\/npm install ${NODE_PACKAGES}; fi; if [[ ! -z ${UNNODE_PACKAGES} ]]; then \/usr\/local\/bin\/npm uninstall ${UNNODE_PACKAGES}; fi; if [ -f \/home\/container\/package.json ]; then \/usr\/local\/bin\/npm install; fi; \/usr\/local\/bin\/node \/home\/container\/${MAIN_FILE}",
    "config": {
        "files": "{}",
        "startup": "{\r\n    \"done\": [\r\n        \"cambia este texto 1\",\r\n        \"cambia este texto 2\"\r\n    ]\r\n}",
        "logs": "{}",
        "stop": "^^C"
    },
    "scripts": {
        "installation": {
            "script": "#!\/bin\/bash\r\n# \ud83d\ude80 Script de instalaci\u00f3n de aplicaci\u00f3n NodeJS\r\n# \ud83d\udcc2 Archivos del servidor: \/mnt\/server\r\n\r\nset -e  # \u26a0\ufe0f Detiene el script en el primer error\r\nset -o pipefail  # \u26a0\ufe0f Detecta errores en pipelines\r\n\r\nDEBUG=true  # \ud83d\udd0d Activa o desactiva debug. Cambia a \"false\" para desactivar.\r\nif [ \"$DEBUG\" = \"true\" ]; then\r\n    set -x  # Imprime cada comando antes de ejecutarlo\r\nfi\r\n\r\necho \"\ud83d\udd27 Actualizando repositorios...\"\r\napt update\r\necho \"\u2705 Repositorios actualizados.\"\r\n\r\necho \"\ud83d\udee0\ufe0f Instalando dependencias esenciales...\"\r\napt install -y git curl jq file unzip make gcc g++ python3 python3-dev python3-pip libtool\r\necho \"\u2705 Dependencias instaladas.\"\r\n\r\necho \"\ud83d\uddd1\ufe0f Desinstalando versiones antiguas de NodeJS y npm...\"\r\napt remove -y nodejs npm || true\r\nrm -rf \/usr\/local\/bin\/node \/usr\/local\/bin\/npm \/usr\/local\/lib\/node_modules\r\necho \"\u2705 NodeJS y npm eliminados.\"\r\n\r\necho \"\ud83d\udce6 Instalando NodeJS 20...\"\r\ncurl -fsSL https:\/\/deb.nodesource.com\/setup_20.x | bash -\r\napt install -y nodejs\r\necho \"\u2705 NodeJS instalado. Versi\u00f3n: $(node -v)\"\r\n\r\necho \"\ud83c\udd99 Actualizando npm a la \u00faltima versi\u00f3n...\"\r\nnpm install npm@latest --location=global\r\necho \"\u2705 npm actualizado. Versi\u00f3n: $(npm -v)\"\r\n\r\necho \"\ud83d\udcc2 Preparando el directorio \/mnt\/server...\"\r\nmkdir -p \/mnt\/server\r\ncd \/mnt\/server\r\necho \"\u2705 Directorio listo.\"\r\n\r\nif [ \"${USER_UPLOAD}\" == \"true\" ] || [ \"${USER_UPLOAD}\" == \"1\" ]; then\r\n    echo \"\ud83d\udc4b Se detect\u00f3 subida de archivos del usuario. Saliendo del script.\"\r\n    exit 0\r\nfi\r\n\r\necho \"\ud83d\udd17 Preparando la direcci\u00f3n del repositorio Git...\"\r\nif [[ ${GIT_ADDRESS} != *.git ]]; then\r\n    GIT_ADDRESS=${GIT_ADDRESS}.git\r\nfi\r\n\r\nif [ -z \"${USERNAME}\" ] && [ -z \"${ACCESS_TOKEN}\" ]; then\r\n    echo \"\ud83d\udd13 Usando conexi\u00f3n an\u00f3nima al repositorio.\"\r\nelse\r\n    echo \"\ud83d\udd10 Usando autenticaci\u00f3n con usuario y token.\"\r\n    GIT_ADDRESS=\"https:\/\/${USERNAME}:${ACCESS_TOKEN}@$(echo -e ${GIT_ADDRESS} | cut -d\/ -f3-)\"\r\nfi\r\n\r\n## Clonar o actualizar el repositorio\r\nif [ \"$(ls -A \/mnt\/server)\" ]; then\r\n    echo \"\ud83d\udcc1 El directorio \/mnt\/server no est\u00e1 vac\u00edo.\"\r\n    if [ -d .git ]; then\r\n        echo \"\ud83d\udd0d El directorio .git existe.\"\r\n        if [ -f .git\/config ]; then\r\n            echo \"\ud83d\udd04 Actualizando el repositorio existente...\"\r\n            git pull\r\n            echo \"\u2705 Repositorio actualizado.\"\r\n        else\r\n            echo \"\u26a0\ufe0f Archivos encontrados sin configuraci\u00f3n git. Saliendo.\"\r\n            exit 10\r\n        fi\r\n    fi\r\nelse\r\n    echo \"\ud83d\udce6 Clonando repositorio en \/mnt\/server...\"\r\n    if [ -z ${BRANCH} ]; then\r\n        echo \"\ud83c\udf3f Clonando rama predeterminada...\"\r\n        git clone ${GIT_ADDRESS} .\r\n    else\r\n        echo \"\ud83c\udf3f Clonando la rama  ${BRANCH} ...\"\r\n        git clone --single-branch --branch ${BRANCH} ${GIT_ADDRESS} .\r\n    fi\r\n    echo \"\u2705 Repositorio clonado.\"\r\nfi\r\n\r\necho \"\ud83d\udce5 Instalando paquetes NodeJS...\"\r\nif [[ ! -z ${NODE_PACKAGES} ]]; then\r\n    echo \"\ud83d\udce6 Instalando paquetes adicionales: ${NODE_PACKAGES}\"\r\n    npm install ${NODE_PACKAGES}\r\nfi\r\n\r\nif [ -f \/mnt\/server\/package.json ]; then\r\n    echo \"\ud83d\udce6 Instalando dependencias de producci\u00f3n desde package.json...\"\r\n    npm install --production\r\n    echo \"\u2705 Dependencias instaladas.\"\r\nelse\r\n    echo \"\u26a0\ufe0f No se encontr\u00f3 package.json. Omitiendo instalaci\u00f3n.\"\r\nfi\r\n\r\necho \"\ud83c\udf89 Instalaci\u00f3n completa. \u00a1Disfruta tu aplicaci\u00f3n NodeJS!\"\r\nexit 0",
            "container": "node:18-bullseye-slim",
            "entrypoint": "bash"
        }
    },
    "variables": [
        {
            "name": "Direcci\u00f3n del Repositorio Git",
            "description": "Repositorio de GitHub para clonar\r\n\r\nEj: https:\/\/github.com\/parkervcp\/repo_name",
            "env_variable": "GIT_ADDRESS",
            "default_value": "https:\/\/github.com\/FzTeis\/Sylphiette",
            "user_viewable": true,
            "user_editable": false,
            "rules": "nullable|string",
            "field_type": "text"
        },
        {
            "name": "Rama de Instalaci\u00f3n",
            "description": "La rama para instalar.",
            "env_variable": "BRANCH",
            "default_value": "",
            "user_viewable": true,
            "user_editable": true,
            "rules": "nullable|string",
            "field_type": "text"
        },
        {
            "name": "Actualizaci\u00f3n Autom\u00e1tica",
            "description": "Extraer los \u00faltimos archivos al iniciar cuando se usa un repositorio GitHub.",
            "env_variable": "AUTO_UPDATE",
            "default_value": "1",
            "user_viewable": true,
            "user_editable": true,
            "rules": "required|boolean",
            "field_type": "text"
        },
        {
            "name": "Archivo Principal",
            "description": "El archivo que inicia la aplicaci\u00f3n.\r\nPuede ser .js o .ts",
            "env_variable": "MAIN_FILE",
            "default_value": "index.js",
            "user_viewable": true,
            "user_editable": true,
            "rules": "required|string|max:16",
            "field_type": "text"
        },
        {
            "name": "Nombre de Usuario Git",
            "description": "Nombre de usuario para autenticar con git.",
            "env_variable": "USERNAME",
            "default_value": "",
            "user_viewable": true,
            "user_editable": true,
            "rules": "nullable|string",
            "field_type": "text"
        },
        {
            "name": "Token de Acceso Git",
            "description": "Contrase\u00f1a para usar con git.\r\n\r\nEs recomendable usar un Token de Acceso Personal.\r\nhttps:\/\/github.com\/settings\/tokens\r\nhttps:\/\/gitlab.com\/-\/profile\/personal_access_tokens",
            "env_variable": "ACCESS_TOKEN",
            "default_value": "",
            "user_viewable": true,
            "user_editable": true,
            "rules": "nullable|string",
            "field_type": "text"
        },
        {
            "name": "Argumentos Adicionales",
            "description": "Cualquier argumento extra para nodejs o ts-node",
            "env_variable": "NODE_ARGS",
            "default_value": "",
            "user_viewable": true,
            "user_editable": true,
            "rules": "nullable|string|max:64",
            "field_type": "text"
        },
        {
            "name": "Paquetes Node adicionales",
            "description": "Instalar paquetes adicionales de node.\r\n\r\nUsa espacios para separar.",
            "env_variable": "NODE_PACKAGES",
            "default_value": "",
            "user_viewable": true,
            "user_editable": true,
            "rules": "nullable|string",
            "field_type": "text"
        },
        {
            "name": "Desinstalar paquetes Node",
            "description": "Desinstalar paquetes node.\r\n\r\nUsa espacios para separar.",
            "env_variable": "UNNODE_PACKAGES",
            "default_value": "",
            "user_viewable": true,
            "user_editable": true,
            "rules": "nullable|string",
            "field_type": "text"
        },
        {
            "name": "Archivos Subidos por el Usuario",
            "description": "Omitir todo el proceso de instalaci\u00f3n si dejas que un usuario suba archivos.\r\n\r\n0 = falso (por defecto)\r\n1 = verdadero",
            "env_variable": "USER_UPLOAD",
            "default_value": "0",
            "user_viewable": true,
            "user_editable": true,
            "rules": "required|boolean",
            "field_type": "text"
        }
    ]
}
