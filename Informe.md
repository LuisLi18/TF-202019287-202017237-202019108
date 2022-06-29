# Encontrar la ruta más corta
WV71 - TRABAJO FINAL DE COMPLEJIDAD ALGORÍTMICA 2022

#### Integrantes
 - Diego Alexander Porta Ñaña
 - Diego Alonso de la Flor Armas
 - Luis Ramón Li Tang

## Introducción
El problema del “Waze” plantea la siguiente interrogante: dada un mapa de la ciudad de Lima, 
una persona se quiere trasladar de un punto a otro por esa zona teniendo en cuenta la optimización de recursos, 
entonces ¿Cómo podemos realizar una búsqueda de la ruta más corta entre dos lugares de la ciudad de Lima? 
Este es un problema cotidiano y complejo dentro de la optimización combinatoria, 
muy importante en la investigación de operaciones y en la ciencia de la computación.

El algoritmo para encontrar la ruta más corta es de suma importancia, 
ya que te permite encontrar las ubicaciones físicas para un traslado hacia otro sitio de manera eficiente. 
Para ser eficiente se requiere establecer y analizar consideraciones como el tiempo y la zona geográfica, entre otras cosas, 
que se detallarán con mayor detalle más adelante.

Usaremos el lenguaje de programación Python ejecutándolo en Google Colab. También, 
para el aporte de la implementación emplearemos librerías principales como folium para generar 
los mapas de la ciudad de Lima, graphviz para la creación de visualización de nodos.

## Objetivos
Los objetivos específicos son construir un grafo, el cual identifique las calles solicitadas y 
las intersecciones de una ciudad. Por otro lado, realizar una búsqueda de la ruta más corta utilizando el algoritmo de Dijkstra.
Con ello, se crea una interfaz gráfica para que el usuario pueda usar la consola de forma más intuitiva y didáctica.

# Marco Teórico

## Algoritmo de Dijkstra
El algoritmo de Dijkstra es un algoritmo para la determinación del camino más corto, dado un vértice origen, hacia el resto de los vértices en un grafo que tiene pesos en cada arista.

La complejidad computacional del algoritmo de Dijkstra se puede calcular contando las operaciones realizadas:

- El algoritmo consiste en n-1 iteraciones, como máximo. En cada iteración, se añade un vértice al conjunto distinguido.

- En cada iteración, se identifica el vértice con la menor etiqueta entre los que no están en Sk. El número de estas operaciones está acotado por n-1.

- Además, se realizan una suma y una comparación para actualizar la etiqueta de cada uno de los vértices que no están en Sk

- Luego, en cada iteración se realizan a lo sumo 2(n-1) operaciones.

Entonces, el algoritmo de Dijkstra realiza O(n^2) operaciones (sumas y comparaciones) para determinar la longitud del camino más corto entre dos vértices de un grafo ponderado simple, conexo y no dirigido con n vértices.

## Código Principal del Dijkstra
<pre>
class Vertice:
	def __init__(self, i):
		self.id = i
		self.vecinos = []
		self.visitado = False
		self.padre = None
		self.costo = float('inf')
		self.x = float('inf')
		self.y = float('inf')

	def agregarVecino(self, v, p, x, y):
		#print(v)
		#if v not in self.vecinos:
		self.vecinos.append([v, p, x, y])

class Grafica:
	def __init__(self):
		self.vertices = {}

	def agregarVertice(self, id):
		if id not in self.vertices:
			self.vertices[id] = Vertice(id)

	def agregarArista(self, a, b, p, x, y):
		if a in self.vertices and b in self.vertices:
			self.vertices[a].agregarVecino(b, p, x, y)
			#self.vertices[b].agregarVecino(a, p)

	def imprimirGrafica(self):
		for v in self.vertices:
			print("El costo del vértice "+str(self.vertices[v].id)+" es "+ str(self.vertices[v].costo)+" llegando desde "+str(self.vertices[v].padre))
			
	
	def camino(self, a, b):
		camino = []
		camino_x = []
		camino_y = []
		actual = b
		llegada = self.vertices[a].padre
		x = b
		y = b
		while actual != llegada:
			camino.insert(0, actual)
			x = self.vertices[actual].x
			camino_x.insert(0, x)
			y = self.vertices[actual].y
			camino_y.insert(0, y)
			actual = self.vertices[actual].padre
		return [camino, self.vertices[b].costo, camino_x, camino_y]

	def minimo(self, l):
		if len(l) > 0:
			m = self.vertices[l[0]].costo
			v = l[0]
			for e in l:
				if m > self.vertices[e].costo:
					m = self.vertices[e].costo
					v = e
			return v
		return None

	def dijkstra(self, a):
		if a in self.vertices:
			# 1 y 2
			self.vertices[a].costo = 0
			actual = a
			noVisitados = []
			
			for v in self.vertices:
				if v != a:
					self.vertices[v].costo = float('inf')
					#self.vertices[v].x = float('inf')
					#self.vertices[v].y = float('inf')
				self.vertices[v].padre = None
				noVisitados.append(v)

			while len(noVisitados) > 0:
				#3
				for vec in self.vertices[actual].vecinos:
					if self.vertices[vec[0]].visitado == False:
						# 3.a
						if self.vertices[actual].costo + vec[1] < self.vertices[vec[0]].costo:
							#print(vec[2])
							self.vertices[vec[0]].costo = self.vertices[actual].costo + vec[1]
							self.vertices[vec[0]].padre = actual
							self.vertices[actual].x = vec[2]
							self.vertices[actual].y = vec[3]
							#print(self.vertices[vec[0]].x)

				# 4
				self.vertices[actual].visitado = True
				noVisitados.remove(actual)

				# 5 y 6
				actual = self.minimo(noVisitados)
		else:
			return False
</pre>

## Conclusiones
En conclusión desarrollamos un sistema el cual nos permita encontrar la ruta más corta usando el algoritmo de Dijkstra entre dos puntos de cualquier parte de la ciudad de Lima, de forma que estará representada por un grafo previamente construido y codificado que representan la ciudad. Además, trabajamos utilizando la responsabilidad y la ética profesional para la elaboración de nuestro trabajo final.
En cuanto a las limitaciones del trabajo realizado, se ha insertado una cierta cantidad de datos para el uso del algoritmo debido a su tiempo de ejecución prolongada. Además, los mapas que muestran la vista general y el camino elegido son mostrados solamente en Google Colab y no en otro sitio. De igual forma con el grafo unidireccional elaborado.

## Bibliografía
EcuRed. (s. f.). Algoritmo de Dijkstra - EcuRed. Algoritmo de Dijkstra. Recuperado 12 de mayo de 2021, de https://www.ecured.cu/Algoritmo_de_Dijkstra

Gómez, E. (2017, 14 septiembre). Mapa de los distritos de Lima, Perú. Astelus. Recuperado 12 de mayo de 2022, de https://astelus.com/mapa-%E2%80%93-peru/mapa-de-los-distritos-de-lima-peru/

LICAD [ LICAD - Facultad de Ingeniería UNAM]. (2020, 19 septiembre). 13 .Algoritmo de Dijkstra en Python [Vídeo]. YouTube. https://www.youtube.com/watch?v=wYyrMnfPmMw

Lima. (2021, 30 julio). Municipalidad Metropolitana de Lima. Recuperado 10 de junio de 2022, de https://www.munlima.gob.pe/lima/ 
