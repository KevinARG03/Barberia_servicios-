# Barberia_servicios-
#DEBEN DE ESTAR EN CARPETAS SEPARADAS 
#index.php
<?php
session_start();

// Datos de servicios
$servicios = array(
    array(
        'id' => 1,
        'nombre' => 'CORTE DE PELO',
        'precio' => 150,
        'descripcion' => 'Un corte moderno y personalizado, adaptado a tu estilo.',
        'icono' => '✂️'
    ),
    array(
        'id' => 2,
        'nombre' => 'AFEITADO',
        'precio' => 140,
        'descripcion' => 'Afeitado clásico con toalla caliente y productos de calidad para una piel suave.',
        'icono' => '🧔'
    ),
    array(
        'id' => 3,
        'nombre' => 'CORTE DE BARBA',
        'precio' => 150,
        'descripcion' => 'Diseño y perfilado de barba para un look impecable.',
        'icono' => '🧔‍♂️'
    ),
    array(
        'id' => 4,
        'nombre' => 'CORTE DE PELO PARA NIÑOS',
        'precio' => 100,
        'descripcion' => 'Corte rápido, seguro y divertido para los más pequeños.',
        'icono' => '👦'
    ),
    array(
        'id' => 5,
        'nombre' => 'AFEITADO CARACTERÍSTICO',
        'precio' => 180,
        'descripcion' => 'Afeitado tradicional con navaja, para un acabado profesional y detallado.',
        'icono' => '✏️'
    ),
    array(
        'id' => 6,
        'nombre' => 'PINTADO DE UÑAS',
        'precio' => 190,
        'descripcion' => 'Aplicación de esmalte con color a elección, para manos impecables.',
        'icono' => '💅'
    )
);

$pagina = isset($_GET['pagina']) ? $_GET['pagina'] : 'inicio';
?>
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Fashion Hair - Servicios de Peluquería</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <!-- HEADER -->
    <header class="header">
        <div class="container-header">
            <div class="logo">
                <span class="logo-icon">✂️</span>
                <h1>Fashion Hair</h1>
            </div>
            <nav class="nav-top">
                <button class="btn-notificacion">🔔</button>
                <div class="user-profile">
                    <img src="https://via.placeholder.com/40" alt="Usuario">
                    <span>Jose</span>
                </div>
            </nav>
        </div>
    </header>

    <div class="container-main">
        <!-- SIDEBAR -->
        <aside class="sidebar">
            <div class="menu-principal">
                <h3>MENÚ PRINCIPAL</h3>
                <ul>
                    <li><a href="?pagina=inicio" class="menu-item <?php echo $pagina === 'inicio' ? 'active' : ''; ?>">🏠 Inicio</a></li>
                    <li><a href="?pagina=galeria" class="menu-item <?php echo $pagina === 'galeria' ? 'active' : ''; ?>">🖼️ Galería</a></li>
                    <li><a href="?pagina=servicios" class="menu-item <?php echo $pagina === 'servicios' ? 'active' : ''; ?>">💼 Servicios</a></li>
                    <li><a href="?pagina=cita" class="menu-item <?php echo $pagina === 'cita' ? 'active' : ''; ?>">📅 Agendar Cita</a></li>
                </ul>
            </div>
            <div class="horario">
                <h3>Horario de Barbería</h3>
                <p class="estado-abierto">ABIERTO</p>
            </div>
        </aside>

        <!-- CONTENIDO PRINCIPAL -->
        <main class="content">
            <?php
            if ($pagina === 'servicios') {
                include 'paginas/servicios.php';
            } elseif ($pagina === 'galeria') {
                include 'paginas/galeria.php';
            } elseif ($pagina === 'cita') {
                include 'paginas/cita.php';
            } else {
                include 'paginas/inicio.php';
            }
            ?>
        </main>
    </div>

    <!-- FOOTER -->
    <footer class="footer">
        <div class="footer-content">
            <p>© Fashion Hair | <a href="#">SOPORTE</a> | <a href="#">DOCUMENTACIÓN</a> | <a href="#">PRIVACIDAD</a></p>
            <p>WEB - v2.4.0</p>
        </div>
    </footer>
</body>
</html>
#procesar_cita.php
<?php
// Procesar formulario de cita
if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    $nombre = htmlspecialchars($_POST['nombre']);
    $telefono = htmlspecialchars($_POST['telefono']);
    $email = htmlspecialchars($_POST['email']);
    $servicio = htmlspecialchars($_POST['servicio']);
    $fecha = htmlspecialchars($_POST['fecha']);
    $hora = htmlspecialchars($_POST['hora']);
    
    $cita = "Nombre: $nombre | Teléfono: $telefono | Email: $email | Servicio: $servicio | Fecha: $fecha | Hora: $hora\n";
    
    file_put_contents('citas.txt', $cita, FILE_APPEND);
    
    header('Location: ?pagina=inicio&mensaje=cita_agendada');
    exit();
}
?> 
#Servicios.php
<section class="servicios-section">
    <h2 class="titulo-seccion">SERVICIOS</h2>
    <p class="subtitulo">Puedes ver los distintos servicios que manejamos</p>
    
    <div class="servicios-grid">
        <?php foreach ($servicios as $servicio): ?>
        <div class="servicio-card">
            <div class="servicio-icono">
                <?php echo $servicio['icono']; ?>
            </div>
            <h3><?php echo $servicio['nombre']; ?></h3>
            <p class="precio">$<?php echo $servicio['precio']; ?></p>
            <p class="descripcion"><?php echo $servicio['descripcion']; ?></p>
            <button class="btn-agendar" onclick="agendarServicio(<?php echo $servicio['id']; ?>, '<?php echo $servicio['nombre']; ?>')">
                Agendar
            </button>
        </div>
        <?php endforeach; ?>
    </div>
</section>
#seccion_citas
<section class="cita-section">
    <h2 class="titulo-seccion">AGENDAR CITA</h2>
    <form class="form-cita" method="POST" action="procesar_cita.php">
        <div class="form-group">
            <label for="nombre">Nombre Completo:</label>
            <input type="text" id="nombre" name="nombre" required>
        </div>
        
        <div class="form-group">
            <label for="telefono">Teléfono:</label>
            <input type="tel" id="telefono" name="telefono" required>
        </div>
        
        <div class="form-group">
            <label for="email">Email:</label>
            <input type="email" id="email" name="email" required>
        </div>
        
        <div class="form-group">
            <label for="servicio">Servicio:</label>
            <select id="servicio" name="servicio" required>
                <option value="">Selecciona un servicio</option>
                <?php foreach ($servicios as $servicio): ?>
                    <option value="<?php echo $servicio['id']; ?>">
                        <?php echo $servicio['nombre']; ?> - $<?php echo $servicio['precio']; ?>
                    </option>
                <?php endforeach; ?>
            </select>
        </div>
        
        <div class="form-group">
            <label for="fecha">Fecha:</label>
            <input type="date" id="fecha" name="fecha" required>
        </div>
        
        <div class="form-group">
            <label for="hora">Hora:</label>
            <input type="time" id="hora" name="hora" required>
        </div>
        
        <button type="submit" class="btn-enviar">Agendar Cita</button>
    </form>
</section>
#inicio.php 
<section class="inicio-section">
    <div class="bienvenida">
        <h2>Bienvenido a Fashion Hair</h2>
        <p>Tu barbería de confianza con los mejores servicios</p>
    </div>
</section>
#Galeria
<section class="galeria-section">
    <h2 class="titulo-seccion">GALERÍA</h2>
    <div class="galeria-grid">
        <div class="galeria-item">
            <img src="https://www.facebook.com/share/1ArDQwdGhg/?mibextid=wwXIfr/300x300" alt="Corte 1">
        </div>
        <div class="galeria-item">
            <img src="https://www.facebook.com/share/1ArDQwdGhg/?mibextid=wwXIfr/300x300" alt="Corte 2">
        </div>
        <div class="galeria-item">
            <img src="https://www.facebook.com/share/1ArDQwdGhg/?mibextid=wwXIfr/300x300" alt="Corte 3">
        </div>
        <div class="galeria-item">
            <img src="https://www.facebook.com/share/1ArDQwdGhg/?mibextid=wwXIfr/300x300" alt="Corte 4">
        </div>
    </div>
</section>
#ESTILOS
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background-color: #f5f5f5;
    color: #333;
}

/* HEADER */
.header {
    background-color: #fff;
    padding: 15px 0;
    box-shadow: 0 2px 5px rgba(0,0,0,0.1);
    position: sticky;
    top: 0;
    z-index: 100;
}

.container-header {
    max-width: 1400px;
    margin: 0 auto;
    padding: 0 20px;
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.logo {
    display: flex;
    align-items: center;
    gap: 10px;
}

.logo-icon {
    font-size: 32px;
}

.logo h1 {
    color: #e91e63;
    font-size: 24px;
    font-weight: bold;
}

.nav-top {
    display: flex;
    align-items: center;
    gap: 20px;
}

.btn-notificacion {
    background: none;
    border: none;
    font-size: 24px;
    cursor: pointer;
}

.user-profile {
    display: flex;
    align-items: center;
    gap: 10px;
}

.user-profile img {
    width: 40px;
    height: 40px;
    border-radius: 50%;
}

/* CONTENEDOR PRINCIPAL */
.container-main {
    max-width: 1400px;
    margin: 0 auto;
    padding: 20px;
    display: grid;
    grid-template-columns: 200px 1fr;
    gap: 20px;
    min-height: calc(100vh - 200px);
}

/* SIDEBAR */
.sidebar {
    background-color: #fff;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 2px 5px rgba(0,0,0,0.1);
    height: fit-content;
}

.menu-principal h3,
.horario h3 {
    color: #e91e63;
    font-size: 14px;
    margin-bottom: 15px;
    font-weight: bold;
}

.menu-principal ul {
    list-style: none;
}

.menu-item {
    display: block;
    padding: 12px 10px;
    color: #333;
    text-decoration: none;
    border-radius: 5px;
    transition: all 0.3s;
    margin-bottom: 5px;
}

.menu-item:hover {
    background-color: #f0f0f0;
}

.menu-item.active {
    background-color: #001a7a;
    color: #fff;
}

.horario {
    margin-top: 30px;
    padding-top: 20px;
    border-top: 1px solid #ddd;
}

.estado-abierto {
    background-color: #e91e63;
    color: white;
    padding: 10px;
    text-align: center;
    border-radius: 5px;
    font-weight: bold;
    font-size: 14px;
}

/* CONTENIDO */
.content {
    background-color: #fff;
    padding: 30px;
    border-radius: 8px;
    box-shadow: 0 2px 5px rgba(0,0,0,0.1);
}

.titulo-seccion {
    color: #333;
    font-size: 28px;
    margin-bottom: 10px;
    font-weight: bold;
}

.subtitulo {
    color: #666;
    margin-bottom: 30px;
    font-size: 14px;
}

/* SERVICIOS */
.servicios-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
    gap: 20px;
}

.servicio-card {
    border: 3px solid #ddd;
    border-radius: 8px;
    padding: 20px;
    text-align: center;
    transition: all 0.3s;
    border-left: 4px solid #001a7a;
}

.servicio-card:nth-child(2n) {
    border-left-color: #e91e63;
}

.servicio-card:nth-child(3n) {
    border-left-color: #001a7a;
}

.servicio-card:hover {
    box-shadow: 0 5px 15px rgba(0,0,0,0.2);
    transform: translateY(-5px);
}

.servicio-icono {
    font-size: 48px;
    margin-bottom: 15px;
}

.servicio-card h3 {
    color: #333;
    font-size: 16px;
    margin-bottom: 10px;
    font-weight: bold;
}

.precio {
    color: #e91e63;
    font-size: 24px;
    font-weight: bold;
    margin-bottom: 10px;
}

.descripcion {
    color: #666;
    font-size: 13px;
    margin-bottom: 15px;
    line-height: 1.5;
}

.btn-agendar {
    background-color: #e91e63;
    color: white;
    border: none;
    padding: 10px 20px;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s;
    font-weight: bold;
}

.btn-agendar:hover {
    background-color: #c2185b;
}

/* FORMULARIO CITA */
.form-cita {
    max-width: 500px;
    margin: 30px auto;
}

.form-group {
    margin-bottom: 20px;
}

.form-group label {
    display: block;
    margin-bottom: 8px;
    color: #333;
    font-weight: bold;
}

.form-group input,
.form-group select {
    width: 100%;
    padding: 12px;
    border: 1px solid #ddd;
    border-radius: 5px;
    font-size: 14px;
    font-family: inherit;
}

.form-group input:focus,
.form-group select:focus {
    outline: none;
    border-color: #e91e63;
    box-shadow: 0 0 5px rgba(233, 30, 99, 0.3);
}

.btn-enviar {
    width: 100%;
    padding: 12px;
    background-color: #001a7a;
    color: white;
    border: none;
    border-radius: 5px;
    font-size: 16px;
    font-weight: bold;
    cursor: pointer;
    transition: background-color 0.3s;
}

.btn-enviar:hover {
    background-color: #001245;
}

/* GALERÍA */
.galeria-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
    gap: 15px;
}

.galeria-item {
    border-radius: 8px;
    overflow: hidden;
    box-shadow: 0 2px 8px rgba(0,0,0,0.1);
}

.galeria-item img {
    width: 100%;
    height: 250px;
    object-fit: cover;
    transition: transform 0.3s;
}

.galeria-item:hover img {
    transform: scale(1.05);
}

/* BIENVENIDA */
.bienvenida {
    text-align: center;
    padding: 50px 20px;
}

.bienvenida h2 {
    color: #333;
    font-size: 36px;
    margin-bottom: 15px;
}

.bienvenida p {
    color: #666;
    font-size: 18px;
}

/* FOOTER */
.footer {
    background-color: #333;
    color: #fff;
    padding: 20px;
    text-align: center;
    margin-top: 40px;
}

.footer-content a {
    color: #e91e63;
    text-decoration: none;
    margin: 0 10px;
}

.footer-content a:hover {
    text-decoration: underline;
}

/* RESPONSIVE */
@media (max-width: 768px) {
    .container-main {
        grid-template-columns: 1fr;
    }
    
    .servicios-grid {
        grid-template-columns: 1fr;
    }
    
    .logo h1 {
        font-size: 18px;
    }
    
    .titulo-seccion {
        font-size: 22px;
    }
}
