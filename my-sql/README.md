kubectl exec -it mysql-pod -- mysql -u root -p
CREATE USER 'nuevo_usuario'@'%' IDENTIFIED BY 'nueva_contraseña';

3. Dar permisos (por ejemplo, acceso total a una base de datos):
GRANT ALL PRIVILEGES ON basededatos.* TO 'nuevo_usuario'@'%';

GRANT ALL PRIVILEGES ON *.* TO 'nuevo_usuario'@'%';
