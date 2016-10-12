% Bioinformática y reproducible con `mk`
% Joshua Ismael Haase Hernández
% Xtraining - INMEGEN - 2016-10-12

![La Biología moderna necesita computadoras.](./biotech.png)

---

# Repetir procesos en varios conjuntos de datos.

![¿Scripts?](./script.gif)

## Manejo de errores en scripts...

- Repetirlo todo.
- Agregarle corrección de errores en cada paso.

---

# `mk`

- reproducible.
- modular.
- automático.
- paralelizado.

![`mk` genera un árbol de dependencias.](./mk.png){height=3cm}

---

# Repetir procesos en varios conjuntos de datos con `mk`

Sólo se necesita un comando:

```
mk
```

## Manejo de errores

- Siempre retoma desde donde se quedó.
- Puede detenerse en un paso intermedio.
- Puede explicar el proceso `mk -dep`.

---

# Estructura `mk`

Un `mkfile` sencillo se ve así:

```
objetivo:	prerequisitos-separados-por-espacios
	comando [opciones] $prereq > $target
```

Se usa \alert{<tab>} para indicar los comandos de una *receta*.

Por defecto `mk` hace \alert{el primer objetivo}.

# Receta para compu (`mkfile`)

```
pastel-de-coco:	harina-de-trigo	huevo	azúcar	coco-rallado
	mezclar harina-de-trigo huevo azúcar > molde-de-hornear
	hornear --time=5min molde-de-hornear
	espolvorear coco-rallado molde-de-hornear > $target

coco-rallado:	coco-pelado
	rallar	$prereq > $target

coco-pelado:	coco
	pelar $prereq > $target
	
harina-de-%:	grano-de-%
	moler --finito --con-molino $prereq > $target

```

---

# Un `mkfile` no tan sencillo

```
VARIABLE1="VALOR"

VARIABLE=`{comando que se guanda en variable}
OBJETIVOS=`{comando que dice qué hacer \
usando \
varias líneas}

todo:V:	$OBJETIVOS

objetivo-%:	\
prerequisitos-separados-por-espacios \
prerequisito-usando-comodín-%
	comando [opciones] $prereq > $target
```


# Instalar `mk`

En varias distribuciones linux, es parte de `9base` o `plan9port`:

```
# apt install 9base
```

Se necesita agregar a tu `~/.profile` o `~/.bashrc`:

```
export PATH=$PATH:/usr/lib/plan9/bin
```

# Instalar `mk`

Si tu distro no lo tiene (o no eres admin):

```
git clone https://github.com/9fans/plan9port
cd plan9port
./INSTALL
```

Se necesita agregar a tu `~/.profile` o `~/.bashrc`:

```
export PATH=$PATH:/donde/quedó/plan9/bin
```


# Taller

- Traducir script a `mk`.

- Alineación de muestras y análisis de calidad.
