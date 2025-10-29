### AVISO: Ver este archivo en Code view para que sea entendible, hay cosas que no aparecen en el modo de vista normal

# Paso 0:
Tener la estructura correcta en github



La estructura:
  
  <extra-addons>
    <dummy_module>
      __init__.py
      __manifest__.py
    </dummy_module>
    .gitkeep
  </extra-addons>
  Dockerfile
  README.md
  

Meter en __manifest__.py:

{

    "name": "Dummy Module",
    
    "version": "1.0",
    
    "summary": "Módulo vacío de prueba",
    
    "installable": True,
    
}

Meter en Dockerfile:
# Imagen base Odoo 17
FROM odoo:17
# (Opcional) módulos propios
COPY ./extra-addons /mnt/extra-addons
# Puerto HTTP de Odoo
EXPOSE 8069
# Puerto por defecto de PostgreSQL
ENV PGPORT=5432
# 1) Inicializa la BD indicada en $PGDATABASE si está vacía (stop-after-init)
# 2) Después arranca el servidor normalmente
#
# NOTA: usamos $PGDATABASE para que la inicialización vaya contra esa BD
# y --db-filter la fije para evitar que Odoo “coja” otra por error.
CMD ["bash","-lc", "\
echo '==> Checking/initializing DB $PGDATABASE' && \
odoo -d $PGDATABASE -i base --without-demo=all \
--db_host=$PGHOST --db_port=$PGPORT \
--db_user=$PGUSER --db_password=$PGPASSWORD \
--addons-path=/usr/lib/python3/dist-packages/odoo/addons,/mnt/extra-addons \
--stop-after-init || true; \
echo '==> Starting Odoo server' && \
odoo --db_host=$PGHOST --db_port=$PGPORT \
--db_user=$PGUSER --db_password=$PGPASSWORD \
--addons-path=/usr/lib/python3/dist-packages/odoo/addons,/mnt/extra-addons \
--db-filter=$PGDATABASE \
--dev=all"]

# Paso 1:
Entrar en Render.com e iniciar sesión con GitHub

# Paso 2:
Crear un nuevo proyecto en el dashboard y darle un nombre

# Paso 3:
En el proyecto, hacer click en New->Postgres
Rellenar Nombre, nombre de la BD y un nombre de usuario
Finalmente seleccionar la opcion gratis

# Paso 4:
Ahora ir de nuevo a New->Web Service y buscar con tu usuario de github y te debería detectar Odoo si tenes la estructura correcta

# Paso 5:
Elegir la opcion gratuita. 
crear 3 variables y darle los valores correctos del Postgres:
  - PGHOST: Hostname
  - PGUSER: Username
  - PGPASSWORD: Password
# Paso 6:
Hacer un deploy en Postgres y luego Odoo
