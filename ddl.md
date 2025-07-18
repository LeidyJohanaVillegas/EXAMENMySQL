```
CREATE DATABASE sakilacampus;
USE sakilacampus;

CREATE TABLE IF NOT EXISTS idioma (
    id_idioma TINYINT PRIMARY KEY,
    nombre CHAR(20),
    ultima_actualizacion TIMESTAMP
);

CREATE TABLE IF NOT EXISTS pais (
    id_pais SMALLINT PRIMARY KEY,
    nombre VARCHAR(50),
    ultima_actualizacion TIMESTAMP
);

CREATE TABLE IF NOT EXISTS ciudad (
    id_ciudad SMALLINT PRIMARY KEY,
    nombre VARCHAR(50),
    id_pais SMALLINT,
    ultima_actualizacion TIMESTAMP,
    CONSTRAINT FK_id_pais FOREIGN KEY (id_pais) REFERENCES pais(id_pais)
);

CREATE TABLE IF NOT EXISTS direccion (
    id_direccion SMALLINT PRIMARY KEY,
    direccion VARCHAR(50),
    direccion2 VARCHAR(50),
    distrito VARCHAR(20),
    id_ciudad SMALLINT,
    codigo_postal VARCHAR(10),
    telefono varchar(20),
    ultima_actualizacion TIMESTAMP,
    CONSTRAINT FK_id_ciudad FOREIGN KEY (id_ciudad) REFERENCES ciudad(id_ciudad)
);

CREATE TABLE IF NOT EXISTS actor (
    id_actor SMALLINT PRIMARY KEY,
    nombre VARCHAR(45),
    apellidos VARCHAR(45),
    ultima_actualizacion TIMESTAMP
);

CREATE TABLE IF NOT EXISTS categoria (
    id_categoria INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(25),
    ultima_actualizacion TIMESTAMP
);

CREATE TABLE IF NOT EXISTS almacen (
    id_almacen INT AUTO_INCREMENT PRIMARY KEY,
    id_empleado_jefe TINYINT,
    id_direccion SMALLINT,
    ultima_actualizacion TIMESTAMP,
    FOREIGN KEY (id_direccion) REFERENCES direccion(id_direccion)
);

CREATE TABLE IF NOT EXISTS empleado (
    id_empleado INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(45),
    apellidos VARCHAR(45),
    id_direccion SMALLINT,
    imagen blob,
    email VARCHAR(50),
    id_almacen INT,
    activo TINYINT(1),
    username VARCHAR(16),
    password VARCHAR(40),
    ultima_actualizacion TIMESTAMP,
    CONSTRAINT FK_id_direccion FOREIGN KEY (id_direccion) REFERENCES direccion(id_direccion),
    CONSTRAINT id_almacen_FK FOREIGN KEY (id_almacen) REFERENCES almacen(id_almacen)
);

CREATE TABLE IF NOT EXISTS pelicula (
    id_pelicula SMALLINT PRIMARY KEY,
    titulo VARCHAR(255),
    descripcion TEXT,
    anyo_lanzamiento YEAR,
    id_idioma TINYINT,
    id_idioma_original TINYINT,
    duracion_alquiler TINYINT,
    rental_rate DECIMAL(4,2),
    duracion SMALLINT,
    replacement_cost DECIMAL(5,2),
    clasificacion ENUM('G','PG','PG-13', 'R', 'NC-17'),
    caracteristicas_especiales set('Trailers', 'Commentaries', 'Deleted Scenes', 'Behind the Scenes'),
    ultima_actualizacion TIMESTAMP,
    FOREIGN KEY (id_idioma) REFERENCES idioma(id_idioma),
    FOREIGN KEY (id_idioma_original) REFERENCES idioma(id_idioma)
);

CREATE TABLE IF NOT EXISTS pelicula_actor (
    id_actor SMALLINT PRIMARY KEY,
    id_pelicula SMALLINT,
    ultima_actualizacion TIMESTAMP,
    CONSTRAINT fk_id_actor FOREIGN KEY (id_actor) REFERENCES actor(id_actor),
    CONSTRAINT fk_id_pelicula FOREIGN KEY (id_pelicula) REFERENCES pelicula(id_pelicula)
);

CREATE TABLE IF NOT EXISTS pelicula_categoria (
    id_pelicula SMALLINT PRIMARY KEY,
    id_categoria INT,
    ultima_actualizacion TIMESTAMP,
    PRIMARY KEY (id_pelicula, id_categoria),
    FOREIGN KEY (id_pelicula) REFERENCES pelicula(id_pelicula),
    FOREIGN KEY (id_categoria) REFERENCES categoria(id_categoria)
);

CREATE TABLE IF NOT EXISTS inventario (
    id_inventario INT AUTO_INCREMENT PRIMARY KEY,
    id_pelicula SMALLINT,
    id_almacen INT,
    ultima_actualizacion TIMESTAMP,
    FOREIGN KEY (id_pelicula) REFERENCES pelicula(id_pelicula),
    FOREIGN KEY (id_almacen) REFERENCES almacen(id_almacen)
);

CREATE TABLE IF NOT EXISTS cliente (
    id_cliente SMALLINT PRIMARY KEY,
    id_almacen INT,
    nombre VARCHAR(45),
    apellidos VARCHAR(45),
    email VARCHAR(50),
    id_direccion SMALLINT,
    activo TINYINT(1),
    fecha_creacion DATETIME,
    ultima_actualizacion TIMESTAMP,
    FOREIGN KEY (id_almacen) REFERENCES almacen(id_almacen),
    FOREIGN KEY (id_direccion) REFERENCES direccion(id_direccion)
);

CREATE TABLE If NOT EXISTS alquiler (
    id_alquiler INT AUTO_INCREMENT PRIMARY KEY,
    fecha_alquiler DATETIME,
    id_inventario INT,
    id_cliente SMALLINT,
    fecha_devolucion DATETIME,
    id_empleado INT,
    ultima_actualizacion TIMESTAMP,
    FOREIGN KEY (id_inventario) REFERENCES inventario(id_inventario),
    FOREIGN KEY (id_cliente) REFERENCES cliente(id_cliente),
    FOREIGN KEY (id_empleado) REFERENCES empleado(id_empleado)
);

CREATE TABLE IF NOT EXISTS pago (
    id_pago SMALLINT PRIMARY KEY,
    id_cliente SMALLINT,
    id_empleado INT,
    id_alquiler INT,
    total DECIMAL(5,2),
    fecha_pago DATETIME,
    ultima_actualizacion TIMESTAMP,
    FOREIGN KEY (id_cliente) REFERENCES cliente(id_cliente),
    FOREIGN KEY (id_empleado) REFERENCES empleado(id_empleado),
    FOREIGN KEY (id_alquiler) REFERENCES alquiler(id_alquiler)
);

CREATE TABLE IF NOT EXISTS film_text (
    film_id SMALLINT PRIMARY KEY,
    title VARCHAR(255),
    description TEXT
);
```
