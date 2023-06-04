##VIDEOJUEGO PRUEBA - USANDO LIBRERIA PYGAME

**DESCRIPCION:**
Este proyecto se encuentra desarrollado en base a python empleando la libreria de pygame. La idea principal de este proyecto fue crear un viedeojuego de forma simple y vistosa como para llamar la atencion del usuario.
![hola](https://www.pygame.org/docs/_images/pygame_logo.png)

**Instalacion:**

Para poder ejecutar este proyecto se puede hacer por medio de una herramienta de edicion de texto o ID que funcione con  python. En el caso de este proyecto, se utilizo PYCHARM en su vercion gratuita.

[Enlace descarga PyCharm](http://https://www.jetbrains.com/es-es/pycharm/download/#section=windows "Enlace descarga PyCharm")

 Se requiere la instalacion de python y la libreria pygame, esto se puede hacer mediante la terminal de PYCHARM y utilizando el comando pip install pygame.
 
 [Enlace descarga Pthon](https://www.python.org/downloads/)
 
 -Se utilizo la veriosn 3.9.0 de python.

```python
pip install pygames
```
 
 **Instrucciones de uso:**
 
 -Para ejecutar este proyecto descargaremos el zip y lo extraeremos en una carpeta.
 
 -Desde PYCHARM abrimos la carpeta donde se extrajo el contenido del zip.
 
 -Cuando se cargue el contenido dentro PYCHARM podremos ejecutar el archivo pruebas.py.
 
 -Al ejecutara una ventana que nos permitira juegar el contenido de este proyecto.
 
 ***CODIGO***
 
 ```python
import pygame
import random
import os
import button
```
Se importan las librerias Pygame encargada de la ejecucion de juego,  random utilizada para generar comportatmientos aleatorios en los enemigos, os para mejorar la busqueda de contenido en las carpetas
```
#se llaman los archivos del proyecto
carpeta_juego = os.path.dirname(__file__)

#se definen la carpetas principales
carpeta_imagnes = os.path.join(carpeta_juego,"img")
carpeta_fondo = os.path.join(carpeta_juego, "fondo")
carpeta_musica = os.path.join(carpeta_juego, "musica")
#se defienen sub carpetas
carpeta_caminar = os.path.join(carpeta_imagnes, "caminar2")
carpeta_caminarIZ = os.path.join(carpeta_imagnes, "caminarIZ")
carpeta_quieto = os.path.join(carpeta_imagnes, "quieto")
carpeta_salto = os.path.join(carpeta_imagnes, "salto")
carpeta_sonido = os.path.join(carpeta_imagnes, "sonido")
carpeta_imagenes_explosiones = os.path.join(carpeta_imagnes, 'explosiones')
carpeta_imagenes_caminar = os.path.join(carpeta_imagnes, 'caminar2')
```
Se realiza el llamado de la carpeta que contienen el proyecto para definir variables que contenga la direccion de estas carpeta y asi acceder de forma mas facil y directa al contenido. 
```
# Iniciación de Pygame
pygame.init()
#se definen una variables para estados de juego
game_paused = False
game_start = False
menu_state = "menu"
```
se inicia la biblioteca de pygame para utilizar sus funciones ademas de crar diferentes variables que rempresentan los estados del juego.
```
# Pantalla - ventana
ANCHO = 600
ALTO = 535
PANTALLA = pygame.display.set_mode((ANCHO, ALTO))
pygame.display.set_caption('prueba')
icono=pygame.image.load(os.path.join(carpeta_imagnes, '0.png'))
pygame.display.set_icon(icono)
FPS = 35
```
Se defiene un aLto y un ancho para crear una ventana utilizando la funcio diplay de pygame adicional a esta ventana se le asigan un nombre y un icono para personalizar la ventan donde se ejecutar el juego.
```
# Fondo del juego
fondo = pygame.image.load(os.path.join(carpeta_fondo, 'fondo3.png'))
contol1 = pygame.image.load(os.path.join(controles, 'control.png'))
contol2 = pygame.image.load(os.path.join(controles, 'C_diparo.png'))
```
Se definen 3 imagenes una para hacer de fondo de juego y las otras dos para el menu de opciones mostrando los controles de juego.
```
pygame.mixer.init()
sonido_disparo = pygame.mixer.Sound("musica/sonidodisparo.wav")
kill = pygame.mixer.Sound("musica/sonidokill.wav")
#musica de fondo
pygame.mixer.music.load("musica/track1.mp3")
pygame.mixer.music.play(-1)
pygame.mixer.music.set_volume(0.5)
```
Se defiene los sonido que tendra el juego sindo estos el sonido de disparo y muerte ademas del sonido de ambiente el cuel se ejecuta bucle.
```
# Sonido
sonido_arriba = pygame.image.load('img/sonido/volumen_mas.png')
sonido_abajo = pygame.image.load('img/sonido/volumen_menos.png')
sonido_mute = pygame.image.load('img/sonido/sin-sonido.png')
sonido_max = pygame.image.load('img/sonido/maximo.png')
```
Se definen elemento para el manejo las funciones del sonido ambiente.  
```
# Control de FPS
reloj = pygame.time.Clock()
```
Se defien un variable reloj la cual se encar de manejar los FPS del juego esto asu vez no indicara la rapidez con la que se ejecuta el juego.
```
def recarga_pantalla():
	global x
	# Fondo en movimiento
	x_relativa = x % fondo.get_rect().width
	PANTALLA.blit(fondo, (x_relativa - fondo.get_rect().width, 0))
	if x_relativa < ANCHO:
		PANTALLA.blit(fondo, (x_relativa, 0))
	x -= 5
```
se de fien una variable global x la cual es utilizafda para realizar un desplazamiento en el fondo del juego , esta funcion hace que el fondo se desplace 5 pixles a la izquierda asi mis mientras el fondo se desplaza este se repite por el por el opuesto evitando el barraido de la imagen creando un bucle que simula movimiento. 
```
#colores
AZUL = (0,0,255)
ROJO = (255,0,0)
NEGRO = (0,0,0)
```
Se defiene algunas paletas de coleores para realizar diferente purebas asi mis mismo usadas para los textos.
```
consolas = pygame.font.match_font('consolas')
times = pygame.font.match_font('times')
arial = pygame.font.match_font('arial')
courier = pygame.font.match_font('courier')
```
Se defiene algunas fuentes de la libreria pygame para los textos en el juego.
```
#sistema de puntos
puntuacion = 0
```
Se defiene un variablepuntuacion la cual ira aumentado o decrementando de dependiendo de si el juegador mata un enemigo o es golpeado por este.
```
animacion_explosion1 = {'t1': [], 't2': [], 't3': [], 't4': []}

for x in range(24):
	archivo_explosiones = f'expl_01_00{x:02d}.png'
	imagenes = pygame.image.load(os.path.join(carpeta_imagenes_explosiones, archivo_explosiones)).convert()
	imagenes.set_colorkey(NEGRO)
	imagenes_t1 = pygame.transform.scale(imagenes, (32,32))
	animacion_explosion1['t1'].append(imagenes_t1)
	imagenes_t2 = pygame.transform.scale(imagenes, (64,64))
	animacion_explosion1['t2'].append(imagenes_t2)
	imagenes_t3 = pygame.transform.scale(imagenes, (128, 128))
	animacion_explosion1['t3'].append(imagenes_t3)
	imagenes_t4 = pygame.transform.scale(imagenes, (256, 256))
	animacion_explosion1['t4'].append(imagenes_t4)
```
Utilizando un ciclo for se hace  un barrido de imagenes dentro de la capeta explociones ademas estas son tranformadas en cuatro tamaños diferentes para luego almencenarlos dentro de areglos los cuales seran llamados para realizar una animacion, se hace de esta forma para generar mas vriedad con una sola animacion.
```
def muestra_texto(PANTALLA,fuente,texto,color, dimensiones, x, y):
	tipo_letra = pygame.font.Font(fuente,dimensiones)
	superficie = tipo_letra.render(texto,True, color)
	rectangulo = superficie.get_rect()
	rectangulo.center = (x, y)
	PANTALLA.blit(superficie,rectangulo)
```
Esta funcion es utilizada para crear el texto que trabaja con la variable puentuacion y las fuentes defienidas anterior ment, para mostrar la puntuacion del juegador en patalla.
```
#define fuente
font = pygame.font.SysFont("arialblack", 30)
#define color
TEXT_COL = (255, 255, 255)
def draw_text(text, font, text_col, x, y):
  img = font.render(text, True, text_col)
  PANTALLA.blit(img, (x, y))
```
como con el texto anteriro se definen fuent y color para mostrar textos en pantalla.
```
#imagen botones
resume_img = pygame.image.load("imagenes/button_resume.png").convert_alpha()
options_img = pygame.image.load("imagenes/button_options.png").convert_alpha()
quit_img = pygame.image.load("imagenes/button_quit.png").convert_alpha()
video_img = pygame.image.load('imagenes/button_video.png').convert_alpha()
audio_img = pygame.image.load('imagenes/button_audio.png').convert_alpha()
keys_img = pygame.image.load('imagenes/button_keys.png').convert_alpha()
back_img = pygame.image.load('imagenes/button_back.png').convert_alpha()

#Intancia botones
resume_button = button.Button(200, 125, resume_img, 1)
options_button = button.Button(195, 250, options_img, 1)
quit_button = button.Button(230, 375, quit_img, 1)
video_button = button.Button(226, 75, video_img, 1)
audio_button = button.Button(225, 200, audio_img, 1)
keys_button = button.Button(246, 325, keys_img, 1)
back_button = button.Button(10, 50, back_img, 1)
```
Se llaman las imagenes de los botones desde la carpeta images y  se instancian los mismo para utilizarlos como menu dentro de los diferentes estados del juego.
```
def barra_hp(PANTALLA, x, y, hp):
	largo = 200
	ancho = 25
	calculo_barra =int((jugador.hp / 100) * largo)
	borde = pygame.Rect(x, y, largo , ancho)
	rectangulo = pygame.Rect(x,y,calculo_barra,ancho)
	pygame.draw.rect(PANTALLA,NEGRO,borde, 3)
	pygame.draw.rect(PANTALLA, ROJO, rectangulo)
	PANTALLA.blit(pygame.image.load("img/quieto/1.png"),(10, 15))
	if jugador.hp < 0:
		jugador.hp = 0
```
Se crea la funcion para realizar una barra que mostrara la vida del jugador, par esto se defien un ancho y un alto para crear un ractangurlo con estos parametros se realiza el calcula el porcentaje vida del personaje  con respecto a la barra, para que esta decremente a medida que la salud del personaje disminuye.
```
class Jugador (pygame.sprite.Sprite):
	#sprite del jugador
	def __init__(self):

		#heredados el init
		super().__init__()

		#se carga la imagen del juegador
		self.image = pygame.image.load("img/quieto/0.png")

		#obtener rectangulo jugador
		self.rect = self.image.get_rect()

		#situar al  juegador dentro de la pantalla 
		self.rect.center = (ANCHO // 2, ALTO // 1)

		#Cadencia de disparo
		self.cadencia = 550
		self.ultimo_disparo = pygame.time.get_ticks()

		#salud jugador
		self.hp = 100

		#velocidad incial del jugador
		self.velocidad_x = 0
		self.velocidad_y = 0


	def update(self):

		#velocidad predeterminade en cada bucle
		self.velocidad_x = 0
		self.velocidad_y = 0

		#movimientos cuando presiona las teclas
		teclas = pygame.key.get_pressed()

		#movimiento Izquierda y derecha
		if teclas[pygame.K_a]:
			self.velocidad_x = -10
		if teclas[pygame.K_d]:
			self.velocidad_x = 10

		#tecla  Disparo
		if teclas[pygame.K_c]:
			ahora = pygame.time.get_ticks()

			#condicion de disparo usa candencia
			if ahora - self.ultimo_disparo > self.cadencia:
				jugador.disparo()
				self.ultimo_disparo = ahora

		#Actualiza la posicion en la que esta el jugador
		self.rect.x += self.velocidad_x
		self.rect.y += self.velocidad_y

		#margenes costados para el jugador
		if self.rect.left < 0:
			self.rect.left = 0
		if self.rect.right > ANCHO:
			self.rect.right = ANCHO
		# margenes arriba yabajo para el jugador
		if self.rect.top < 0:
			self.rect.top = 0
		if self.rect.bottom > ALTO:
			self.rect.bottom = ALTO

	#funcion disparo
	def disparo(self):
		bala = Disparos(self.rect.centerx, self.rect.top)
		balas.add(bala)
		sonido_disparo.play()

class Disparos(pygame.sprite.Sprite):
	def __init__(self, x ,y):

		super().__init__()
		#se carga la imaegen que tendra el disparo
		self.image = pygame.image.load("img/disparo/bullet0.png")
		#se crea el rectangulod del disparo
		self.rect = self.image.get_rect()
		#posicion
		self.rect.bottom = y
		self.rect.centerx = x

	def update(self):
		#decrementa en a posicion y
		self.rect.y -= 25
		#si llega al tope de la pantalla desaparece 
		if self.rect.bottom < 0:
			self.kill()
```
Se crea la clase del jugandor  y se definen sus funciones, se le hereda todo lo referente a la clase sprite  con esto pedemos cargar al jugador en la pantlla, darle una poscion inicial y darle movimiento, ademas de crear la clase y la funcion para que este pueda disprar.
```
class Enemigos(pygame.sprite.Sprite):
	#sprite del Enemigo
	def __init__(self):

		#heredados el init
		super().__init__()

		#rectangulo enemigo
		self.image = pygame.image.load("img/enemigo/0.png")

		#Centrar obtener rectangulo enemigo
		self.rect = self.image.get_rect()

		#se le da una posicion y velocidad utilizando random
		#posicion ramdom los enemigos
		self.rect.x = random.randrange(ANCHO)s
		self.velocidad_x = random.randrange(1,10)
		self.velocidad_y = random.randrange(1,10)

		#vida enemigo
		self.hp = 500

	def update(self):
		#actualiza la posicion del enemigo
		self.rect.x += self.velocidad_x
		self.rect.y += self.velocidad_y

		# margen costados para rebote
		if self.rect.left < 0:
			self.velocidad_x += 1
		if self.rect.right > ANCHO:
			self.velocidad_x -= 1
		#margen arriba y abajo para rebote de enemigo
		if self.rect.top < 0:
			self.velocidad_y += 1
		if self.rect.bottom > ALTO:
			self.velocidad_y -= 1
###################################################################
class Enemigos2(pygame.sprite.Sprite):
	#sprite del Enemigo
	def __init__(self):
		#heredados el init
		super().__init__()

		#rectangulo enemigo
		self.image = pygame.image.load("img/enemigo/E2.png")
		#Centrar obtener rectangulo enemigo
		self.rect = self.image.get_rect()

		#posicion ramdom y velocidad enemigos
		self.rect.x = random.randrange(ANCHO)
		self.velocidad_x = random.randrange(1,2)
		self.velocidad_y = random.randrange(1,2)

	def update(self):
		#actualiza la posicion del enemigo
		self.rect.x += self.velocidad_x
		self.rect.y += self.velocidad_y

		# margen costados para rebote
		if self.rect.left < 0:
			self.velocidad_x += 1
		if self.rect.right > ANCHO:
			self.velocidad_x -= 1

		#margen arriba y abajo para rebote de enemigo
		if self.rect.top < 0:
			self.velocidad_y += 1
		if self.rect.bottom > ALTO:
			self.velocidad_y -= 1
###################################################################
class Enemigos3(pygame.sprite.Sprite):
	#sprite del Enemigo
	def __init__(self):
		#heredados el init
		super().__init__()

		#rectangulo enemigo
		self.image = pygame.image.load("img/enemigo/E3.png")

		#Centrar obtener rectangulo jugador
		self.rect = self.image.get_rect()

		#posicion ramdom y velocidad enemigos
		self.rect.x = random.randrange(ANCHO)
		self.velocidad_x = random.randrange(1,5)
		self.velocidad_y = random.randrange(1,5)

		# rectangulo enemigo
		#self.image = pygame.Surface((68, 78))
		#self.image.fill(ROJO)

	def update(self):
		#actualiza la posicion del enemigo
		self.rect.x += self.velocidad_x
		self.rect.y += self.velocidad_y

		# margen costados para rebote
		if self.rect.left < 0:
			self.velocidad_x += 1
		if self.rect.right > ANCHO:
			self.velocidad_x -= 1
		#margen arriba y abajo para rebote de enemigo
		if self.rect.top < 0:
			self.velocidad_y += 1
		if self.rect.bottom > ALTO:
			self.velocidad_y -= 1
###################################################################

class Enemigo4(pygame.sprite.Sprite):
	def __init__(self):
		super().__init__()

		self.image = pygame.image.load("img/enemigo/bee.png")

		self.rect = self.image.get_rect()
		self.rect.x = random.randrange(ANCHO - self.rect.width)
		self.rect.y = self.rect.width
		self.velocidad_y = random.randrange(1, 10)

	def update(self):
		self.rect.y += self.velocidad_y
		if self.rect.top > ALTO:
			self.rect.x = random.randrange(ANCHO - self.rect.width)
			self.rect.y = -self.rect.width
			#ancho
			self.velocidad_y = random.randrange(20, 30)
```
Como con el juegador se crea la clase enemigo y se ereda la clase sprite ademas cargar las imagenes la posicion, velocidad y patron de movimientos, se hace un duplicado de la primera clase enemigos para reutilizarla en los demas. 
```
class Explosiones(pygame.sprite.Sprite):
	#sprite de la explicion
	def __init__(self, centro, dimensiones):
		pygame.sprite.Sprite.__init__(self)
		self.dimensiones = dimensiones
		#se carga el arreglo con animacion
		self.image = animacion_explosion1[self.dimensiones][0]
		#rectangulo imagen
		self.rect = self.image.get_rect()
		#posicion centro
		self.rect.center = centro
		#fotogrma inicial
		self.fotograma = 0
		#cantidad de fotogrmas a cargar
		self.frecuencia_fotograma = 35
		#tiempo fotogrmas
		self.actualizacion = pygame.time.get_ticks()

	def update(self):
		ahora = pygame.time.get_ticks()
		if ahora - self.actualizacion > self.frecuencia_fotograma:
			self.actualizacion = ahora
			self.fotograma += 1
			if self.fotograma == len(animacion_explosion1[self.dimensiones]):
				self.kill()
			else:
				centro = self.rect.center
				self.image = animacion_explosion1[self.dimensiones][self.fotograma]
				self.rect = self.image.get_rect()
				self.rect.center = centro
```
Se  crea la clase explicion para cargar una animacion de explocion cuando los enemigo las balas del juegador impactan con el enemeigo para esto se usan los areeglos antes mencionados para que al cargarlos cada imagen almacenada sea un fotogrma de la animacion.
```
#grupos sprites
sprites = pygame.sprite.Group()
enemigos = pygame.sprite.Group()
enemigos2 = pygame.sprite.Group()
enemigos3 = pygame.sprite.Group()
balas = pygame.sprite.Group()
enemigo4 = pygame.sprite.Group()
explosiones = pygame.sprite.Group()
jugador = Jugador()
sprites.add(jugador)
```
se defiene los grupos de sprite a los que pertenece cada elemento esto con el fin de que cada grupo pueda interactuar con los demas, de esta menera se puede organizar y controlar fácilmente múltiples objetos en el juego.
```
ejecuta = True
# Bucle de acciones y controles
while ejecuta:
	# FPS
	reloj.tick(FPS)
```
Se ejecuta el bucle while y se le asigna una varible para realizar los cambios de estado.
```
	# Bucle del juego
	for event in pygame.event.get():
		if event.type == pygame.KEYDOWN:
			if event.key == pygame.K_SPACE:
				game_paused = True
			if event.key == pygame.K_l:
				game_start = True

		if event.type == pygame.QUIT:
			ejecuta = False

	if game_start == True:

		if game_paused == True:
			if menu_state == "menu":
				if resume_button.draw(PANTALLA):
					game_paused = False
				if options_button.draw(PANTALLA):
					menu_state = "options"
				if quit_button.draw(PANTALLA):
					ejecuta = False
			if menu_state == "options":
				# draw the different options buttons
				draw_text("CONTROLES", font, TEXT_COL, 250, 50)
				PANTALLA.blit(contol1,(50,200))
				PANTALLA.blit(contol2, (300, 200))
				sprites.update()
				balas.update()
				sprites.draw(PANTALLA)
				balas.draw(PANTALLA)
				if back_button.draw(PANTALLA):
					menu_state = "menu"
	else:
		draw_text("Presione SPACE para PAUSAR", font, TEXT_COL, 50, 250)
```
Se creal el bucle for que determina se el juego se esta ejecutando y se crean los diferentes estados del juego por medio de condiciones y se hace el llamado de los diferente botones, images y textos de los menus para que se muestren en pantalla.
```
			sprites.update()
			enemigos.update()
			enemigos2.update()
			enemigos3.update()
			balas.update()
			enemigo4.update()
			explosiones.update()
```
Se cargan los update que actualiza los sprites en la pantalla de no ser asi estos se quedan estaticos pues cargan un solo fotogrma inicial.
```
			if not enemigos and not enemigos2 and not enemigos3 and not enemigo4:

				enemigo = Enemigos()
				enemigos.add(enemigo)


				enemigo2 = Enemigos2()
				enemigos2.add(enemigo2)

				enemigo3 = Enemigos3()
				enemigos3.add(enemigo3)



				enemigos4 = Enemigo4()
				enemigo4.add(enemigos4)
```
Este condicional determina si hay sprites de enemigos cargados en pantalla de no cumlir la condicion carga los enemigo a la pantalla, esto es para la generacion de los priemros enemigos.
```
			muestra_texto(PANTALLA, consolas, str(puntuacion).zfill(7), ROJO, 40, 500, 50)

			barra_hp(PANTALLA, 10, 15, jugador.hp)
```
se hace el llamado a las funciones que muestran la puntuacion y la barra de vida del jugador.
```
#colicines de enemigos y balas
			colision_disparos_amarillos = pygame.sprite.groupcollide(enemigos, balas, False, False,pygame.sprite.collide_circle)

			colision_disparos_verdes = pygame.sprite.groupcollide(enemigos2, balas, True, True,pygame.sprite.collide_circle)

			colision_disparos_azules = pygame.sprite.groupcollide(enemigos3, balas, True, True,pygame.sprite.collide_circle)

			colision_disparos_rojos = pygame.sprite.groupcollide(enemigo4, balas, True, True,pygame.sprite.collide_circle)


#coliciones enemigo jugador 
			colision_pj1 = pygame.sprite.spritecollide(jugador, enemigos, True)

			colision_pj2 = pygame.sprite.spritecollide(jugador, enemigos2, True)

			colision_pj3 = pygame.sprite.spritecollide(jugador, enemigos3, True)

			colision_pj4 = pygame.sprite.spritecollide(jugador, enemigo4, True)

#condiciones de colicion enemigo jugados 
			if colision_pj1:
				#colicon enemigo 1 prierde 100 puntos 
				puntuacion -= 100
				#colicon enemigo 1 prierde 10 de vida  
				jugador.hp -= 10
				#ejecuta sonido muerte
				kill.play()
				#animacion muerte enemigo
				explosion = Explosiones(enemigo.rect.center, 't1')
				explosiones.add(explosion)
				#reaparece enemigo muerto
				enemigo = Enemigos()
				enemigos.add(enemigo)

			if colision_pj2:
				puntuacion -= 50
				jugador.hp -= 10
				kill.play()
				explosion = Explosiones(enemigo2.rect.center, 't1')
				explosiones.add(explosion)
				enemigo2 = Enemigos2()
				enemigos2.add(enemigo2)

			if colision_pj3:
				puntuacion -= 25
				jugador.hp -= 10
				kill.play()
				explosion = Explosiones(enemigo3.rect.center, 't1')
				explosiones.add(explosion)
				enemigo3 = Enemigos3()
				enemigos3.add(enemigo3)

			if colision_pj4:
				puntuacion -= 10
				jugador.hp -= 10
				kill.play()
				explosion = Explosiones(enemigos4.rect.center, 't1')
				explosiones.add(explosion)
				enemigos4 = Enemigo4()
				enemigo4.add(enemigos4)

#si la vida del jugado se agota cierra el juego 
			if jugador.hp <= 0:
				ejecuta = False

#condiciones colicion enmigo balas
			if colision_disparos_amarillos:
			#ejecuta sonido muerte
				kill.play()
			#animacion muerte enemigo
				explosion = Explosiones(enemigo.rect.center, 't3')
				explosiones.add(explosion)
				enemigo.hp -= 10

#si vida de enemigo llega a 0 
			if enemigo.hp <=0:
			#destruye el objeto
				enemigo.kill()
			#incrementa puntucacion 
				puntuacion += 100
			#animacion muerte enemigo mas grande
				explosion = Explosiones(enemigo.rect.center, 't4')
				explosiones.add(explosion)
				r#eaparece enemigo
				enemigo = Enemigos()
				enemigos.add(enemigo)

			if colision_disparos_verdes:
				puntuacion += 50
				kill.play()
				explosion = Explosiones(enemigo2.rect.center, 't3')
				explosiones.add(explosion)
				enemigo2 = Enemigos2()
				enemigos2.add(enemigo2)

			if colision_disparos_azules:
				puntuacion += 25
				kill.play()
				explosion = Explosiones(enemigo3.rect.center, 't3')
				explosiones.add(explosion)
				enemigo3 = Enemigos3()
				enemigos3.add(enemigo3)

			if colision_disparos_rojos:
				puntuacion += 10
				kill.play()
				explosion = Explosiones(enemigos4.rect.center, 't3')
				explosiones.add(explosion)
				enemigos4 = Enemigo4()
				enemigo4.add(enemigos4)
```
Se indican las coliciones del juego tanto de enemigos como del jugador y las balas del mismo,  se ñaden los con dicionales que indican que suscede con cada colision.
```
			sprites.draw(PANTALLA)
			enemigos.draw(PANTALLA)
			enemigos2.draw(PANTALLA)
			enemigos3.draw(PANTALLA)
			balas.draw(PANTALLA)
			enemigo4.draw(PANTALLA)
			explosiones.draw(PANTALLA)
```
Se muestran los imagnes de los sprites en pantalla. 
```
			# Opción tecla pulsada
			keys = pygame.key.get_pressed()
```
indica si una tecla se  esta precionando esta es otra manera de emplear el uso de teclas he imagenes si usar sprites.
```
			# Control del audio
			# Baja volumen
			if keys[pygame.K_9] and pygame.mixer.music.get_volume() > 0.0:
				pygame.mixer.music.set_volume(pygame.mixer.music.get_volume() - 0.01)
				PANTALLA.blit(sonido_abajo, (250, 25))
			elif keys[pygame.K_9] and pygame.mixer.music.get_volume() == 0.0:
				PANTALLA.blit(sonido_mute, (250, 25))

			# Sube volumen
			if keys[pygame.K_0] and pygame.mixer.music.get_volume() < 1.0:
				pygame.mixer.music.set_volume(pygame.mixer.music.get_volume() + 0.01)
				PANTALLA.blit(sonido_arriba, (250, 25))
			elif keys[pygame.K_0] and pygame.mixer.music.get_volume() == 1.0:
				PANTALLA.blit(sonido_max, (250, 25))

			# Desactivar sonido
			elif keys[pygame.K_m]:
				pygame.mixer.music.set_volume(0.0)
				PANTALLA.blit(sonido_mute, (250, 25))

			# Reactivar sonido
			elif keys[pygame.K_COMMA]:
				pygame.mixer.music.set_volume(1.0)
				PANTALLA.blit(sonido_max, (250, 25))
	else:
		draw_text("Presione ENTER para INICIAR", font, TEXT_COL, 50, 250)
```
Se usan diferentes condicionales y he imagenes para manjar el sonido amibiente del juego.

```
	# Actualización de la ventana
	pygame.display.update()
	#Llamada a la función de actualización de la ventana
	recarga_pantalla()



# Salida del juego
pygame.quit()
```
Finalmente se le indica la actualizacion de la pantalla para que en cada bucle esta muestre nuevos fogrmas ademas de la conidcion de salida del bucle en caso de que la variable ejecuta cambie.
