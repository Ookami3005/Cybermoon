# Práctica 2 - Criptografía y Seguridad

> Fernando Romero Cruz - *319314256*

### Reporte

#### Parte 1

> Para esta sección, se utilizó un *Script* simple de *Python* para abrir el archivo y realizar las iteraciones de descifrado hasta dar con aquella que contuviera la cadena `DOS`. Una vez encontrada, se almacenó la llave y se escribio el contenido descifrado con la llave correcta, retirando la cabecera, en el archivo `virus`.

```python
#!/usr/bin/env python

reader = open('57FD6325.VBN','rb')
mis_bytes = reader.read()
llave = 0

for i in range(256):
     temp = ''.join([chr(b^i) for b in mis_bytes])
     if 'DOS' in temp:
        llave = i
		break

print(f'Llave encontrada: {llave}')
desofus = ''.join([chr(b^llave) for b in mis_bytes]).encode()

start = desofus.find(b'MZ')
print(f'Numeros magicos encontrados en el byte: {start}')

writer = open('virus','wb')
writer.write(desofus[start:])
```

Este *Script* nos da una salida como la siguiente, ademas de crear el archivo especificado, que podemos comprobar que efectivamente es un **ejecutable** de *Windows*:

![recupera_virus.png](imagenes/recupera_virus.png)

Tambien podemos comprobar que efectivamente es un virus subiendo el archivo a la página *Virus Total*:

![virustotal.png](imagenes/virustotal.png)

#### Parte 2

> Esta segunda sección si se me dificultó un poquito más. Empecé como recomienda el documento, al realizar un *XOR* entre el criptograma que deseamos recuperar con el resto.

De esta manera sabemos que todos las nuevas cadenas, por la propiedad inversa del *XOR*, son en realidad el resto de **mensajes claros** operados con el mensaje en claro que buscamos.

Sabiendo esto, estuve mucho tiempo intentando figurar como podría apoyarme del dato de los espacios invirtiendo *capitalización* pero sinceramente no me resulto de mucha ayuda.
Investigando un poco, leí que uno de ==los mejores metodos para romper estas encriptaciones *OTP* era intentar descifrar varios segmentos de estas cadenas con palabras que sabemos o esperamos que estén en el mensaje en claro, esperando encontrar palabras con sentido==. Como desconocía cualquier indicio del contenido del mensaje en ese momento, simplemente intente con las palabras más comunes en el español que son adverbios y preposiciones como *"que"*, *"cuando"*, *"pero"*, *"porque"*, *"el"*, y demás palabras similares.

Un problema fue que entre más corta fuera la palabra, más dificil era identificar palabras con sentido en las resultantes. Para esto intente usar las palabras más largas y otra cosa que me fue de mucha ayuda fue agregar espacios al inicio y al final de la palabra, pues regularmente deberían ir separadas todas las palabras, ganando otros 2 caracteres de longitud.


Recordemos que las cadenas resultantes al inicio, son en realidad el **mensaje en claro** que buscamos, operado con *XOR* a cada uno del resto de mensajes en claro. Entonces si lograbamos descubir alguna palabra en el mensaje que buscamos, **teóricamente** deberiamos recibir mediante este código *11* cadenas con sentido, lo cual sería de mucha ayuda ya que si usamos una palabra en un mensaje en claro restante, solo obtendriamos 1 cadena con sentido (la del mensaje buscado).

### Cuestionario

1. ¿Para qué se usa la herramienta XORsearch? https://blog.didierstevens.com/programs/xorsearch/

> Es una utilidad que permite buscar cadenas en archivos **posiblemente** codificados en *XOR*, *ROL* o *ROT*, precisamente realizando varias de estas desencriptaciones y analizando si la cadena buscada se encuentra en esta nueva información.
> 
> Particularmente para *XOR*, prueba todas las posibles llaves de un solo *byte*, de 0 a 255.

2. ¿De cuántos bytes es la cabecera que le agregó el antivirus al malware?

> De ***13536*** *bytes*, pues los números mágicos comienzan en el **13537**.

3. ¿Qué son los números mágicos? (relacionado con archivos)

> Los números mágicos buscados son `0x4d` y `0x5a`, o tambien representable como la cadena `MZ`.

4. ¿Qué es VirusTotal?

> Una base de datos que almacena las información, características y funcionalidades de *Malware* previamente detectado y cargado a ésta página.
> Este *Malware* es identificado principalmente por sus firmas *hash*.

5. De acuerdo a VirusTotal, ¿qué tipo de malware es?

> *VirusTotal* parece indicar que se trata de un **virus tipo troyano**.

6. En la parte 2 de la práctica, ¿a qué obra y a qué autora pertenecen los textos que logró descifrar?

