#Introdución
------------
En este proyecto se verá la realización del sistema solar con Unity, mediante un Universal Render Pipeline, utilizando Shadergraph. Esto se logrará utilizando las diversas técnicas y enseñanzas obtenidas en clase, así como a su vez intentar técnicas distintas como lo son los efectos visuales. El objetivo es realizar un Sistema Solar con mucha semejanza al real.

Teniendo integrado los siguientes puntos:
- Textura
- Normal Map
- Rim Light
- Efectos extra sobre la Superficie
- Orden de los respectivos planetas
- Visual Efects

-------------
#Desarrollo
-------------
Para la creación de la mayoría de los planetas, se utilizó un Shader Lit que forma parte del universal render pipeline, una vez creado el desarrollo del shader se dividió en cuatro segmentos importantes que lo conformarán mediante el uso de nodos. Que vendrían siendo la main texture, el normal map, la Rim Light y el occlussion junto a Visual Effects.

**Main Texture**

**Normal Map**

**Rim Light**

**Occlussion**

**Video Animacion**

[![Watch the video](https://i.ytimg.com/an_webp/YaY5JiCBLmk/mqdefault_6s.webp?du=3000&sqp=CPyv74IG&rs=AOn4CLBw3hVuIiAeDHuuygoQ3d89NQ3-nQ)](https://youtu.be/YaY5JiCBLmk)

**Obtencion de Texturas**
Para obtener la textura base de los planetas, se visitó el sitio Solar System Scope, donde se pueden encontrar texturas reales en resolución 2K de todos los planetas de forma gratuita.

![alt text](https://github.com/Lelmats/Sistema-Solar/blob/main/Images/Foto%201.png)

Sin embargo, no contaban con la mayoría de las texturas normales y ninguna de oclusión ambiental, por lo que se utilizó un programa llamado Crazybump con el que pudimos mapear estas texturas fácilmente. 

En este programa primero se introduce la textura original, y luego el programa pide que elijas una imagen para hacer un mapeado de la textura, en este caso se utilizó la primera figura.

![alt text](https://github.com/Lelmats/Sistema-Solar/blob/main/Images/Foto%202.png)

Una vez seleccionada la figura, se puede pasar a conseguir la textura normal y de oclusión ambiental. Se modifican los sliders para tener una textura más semejante a la de la realidad, y de ahí se exporta directamente en la carpeta de unity, donde se encuentra la textura principal.

![alt text](https://github.com/Lelmats/Sistema-Solar/blob/main/Images/Foto%203.png)

Para realizar la textura principal, se eligió una de las que se descargaron anteriormente. La textura se indica en un Sample Texture 2D, el cual estará conectado con el color base del shader. A esta textura se le aplican varias cosas. La primera es el efecto de Tiling and Offset, el cual se usa para rotar la textura alrededor de una esfera, para simular la rotación del planeta sobre su propio eje. Se tiene que conectar un nodo de multiplicación del tiempo transcurrido con la velocidad de rotación. La fórmula es un despeje de velocidad: V = D/T  a D = V*T.

Este nodo va conectado con un Vector2, que será el que maneje el eje X. Esto es importante, ya que si no se conecta aquí, la textura comenzará a rotar en direcciones no deseadas, por lo que le indicamos que solo se mueva en el eje X.

![alt text](https://github.com/Lelmats/Sistema-Solar/blob/main/Images/Foto%204.png)

Para la textura Normal, se realizó un proceso similar, donde se indica la textura en un Sample Texture 2D, y su UV está conectado con el mismo Tiling and Offset de la textura principal, ya que estas texturas tienen que rotar a la misma velocidad y dirección. Sin embargo, es importante asegurarse de que el tipo de textura esté indicado como Normal y no Default como en la principal, para dar un efecto más convincente. Sirve para añadir profundidad al planeta, y que no sea simplemente una esfera plana. El Sample Texture 2D luego pasa a un filtro donde se puede manejar su fuerza utilizando un slider. Después va conectada al nodo Normal (Tangent Space) del fragmento.

![alt text](https://github.com/Lelmats/Sistema-Solar/blob/main/Images/Foto%205.png)

Un efecto importante para la composición del planeta fue el de un anillo de luz, o Rim Light. Para esto se necesitó utilizar el nodo de Fresnel Effect, el cual necesita un vector3, la dirección de vista, y un Float el cual manejaría la intensidad del anillo. Esto pasa a otro donde se multiplican el efecto anterior con un color HDR, para así poder cambiar el color del anillo fácilmente.  Una vez terminado el efecto, este pasa a conectarse con el nodo de emisión del fragmento.

![alt text](https://github.com/Lelmats/Sistema-Solar/blob/main/Images/Foto%206.png)

El último efecto añadido fue el de oclusión ambiental, el cual añade efectos muy sutiles de gases a la atmósfera del planeta, y su UV está conectada con el mismo Tiling and Offset que las demás. Es una textura que se ubica conecta en el nodo de oclusión ambiental en el fragmento.

![alt text](https://github.com/Lelmats/Sistema-Solar/blob/main/Images/Foto%208.png)

Este es el resultado del planeta:

![alt text](https://github.com/Lelmats/Sistema-Solar/blob/main/Images/Foto%209.png)

**Sol y Anillos, Saturno - Visual Effects**

Para el Sol se usó un “Visual Effect” el cual a continuación se verá lo que se utilizó para la elaboración de este.



Primero seleccionamos el spawn rate, que es la cantidad de veces que aparecerán las partículas por segundo

![alt text](https://github.com/Lelmats/Sistema-Solar/blob/main/Images/Foto%2010.png)



Luego se ve la parte de “Initialize Particles” en la que ponemos el número de partículas que spawneen, o aparecerán en el tiempo elegido anteriormente, abajo de este, está su tiempo de vida, y se tiene A u B.

![alt text](https://github.com/Lelmats/Sistema-Solar/blob/main/Images/Foto%2012.1.png)

Lo siguiente fue escoger su tipo de posición y forma de esta que tomará, pero necesitará una fuerza más adelante, se escogió el surface, por que se vería mejor.

![alt text](https://github.com/Lelmats/Sistema-Solar/blob/main/Images/Foto%2011.png)

Por consiguiente se pasa a *“Update Particles”* aquí escoge la fuerza que necesitarán las partículas para que este tome realmente la forma de la esfera, y sea forzado a esta, modificarla como el radio que tendrá entre otras cosas como la atracción, etc.

![alt text](https://github.com/Lelmats/Sistema-Solar/blob/main/Images/Foto%2012.png)

Lo siguiente es la turbulencia, que se tomó como relativa, se le dio una intensidad y un *“Drag”* que ayudará a dar el efecto de los gases saliendo del sol., la frecuencia en 5 junto a los valores que se pueden observar en la siguiente imagen.


Por último tenemos el *“Output Particle”*, aquí es donde los efectos pueden tomar como en este caso, color, alpha y un blend en cualquiera de estas, y se visualiza de mejor manera.

![alt text](https://github.com/Lelmats/Sistema-Solar/blob/main/Images/Foto%2013.1.png)

Aquí se le añadió tamaño a las partículas, de manera general y luego un Scale XYZ, para que fueran más planas y se apreciara mejor.


![alt text](https://github.com/Lelmats/Sistema-Solar/blob/main/Images/Foto%2013.2.png)

Agregamos un color naranja de base, asi luego poner un color amarillo en el caso de sol, dandole un efecto HDR, en el cual pues nos dará un efecto de iluminado y emisivo. al final un “Blend Alpha” que nos dará otro efecto visual interesante. En el caso de los anillos de Saturno, se cambiaron algunos datos que se pueden ver a continuación.

Como resultado Final se obtiene esto:

![alt text](https://github.com/Lelmats/Sistema-Solar/blob/main/Images/Foto%2014.png)

![alt text](https://github.com/Lelmats/Sistema-Solar/blob/main/Images/Foto%2015.png)

Por último lo que se hizo fue acomodar los planetas alrededor del sol, además de sus distancias, tamaño de cada planeta, está basado en la documentación de la NASA, en su website oficial.

![alt text](https://github.com/Lelmats/Sistema-Solar/blob/main/Images/Foto%2016.png)

---------------

#Conclusión
--------------
Como conclusión, podemos decir que el resultado obtenido fue satisfactorio, se lograron crear modelos muy semejantes a los planetas de la vida real, se respetaron escalas en cuanto a tamaños de planetas y en términos generales, los shaders cumplieron su función, con texturas, normal texture y occlusion, se crearon los planetas, respetando así sus respectivos relieves así como sus colores y apariencia. 

--------------
#Referencias
-----------
- Medidas: </https://solarsystem.nasa.gov/planets/overview/>
- Texturas: </https://www.textures.com/library/>
- Texturas: </https://www.solarsystemscope.com/textures/>

-----------
#Integrantes
-------------
- Max Rivera
- German Pablos
- Jose Daniel Becerra
- Jorge Perez 
