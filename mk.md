% Bioinformática automática y reproducible con `mk`
% Joshua Ismael Haase Hernández
% INMEGEN - 2017-02-23

![La Biología moderna necesita computadoras.](data/curso-mk/biotech.png)

---

## Repetir procesos en varios conjuntos de datos.

![¿Scripts?](data/curso-mk/script.gif)

### Manejo de errores en scripts...

- Repetirlo todo.
- Agregarle corrección de errores en cada paso.

---

## `mk`

- reproducible.
- modular.
- automático.
- paralelizado.

![`mk` genera un árbol de dependencias.](data/curso-mk/mk.png){height=3cm}

---

## Repetir procesos en varios conjuntos de datos con `mk`

Sólo se necesita un comando:

```
mk
```

### Manejo de errores

- Siempre retoma desde donde se quedó.
- Puede detenerse en un paso intermedio.
- Puede explicar el proceso `mk -dep`.

---

## Estructura `mk`

Un `mkfile` sencillo se ve así:

```
objetivo:	prerequisitos-separados-por-espacios
	comando [opciones] $prereq > $target
```

Se usa \alert{<tab>} para indicar los comandos de una *receta*.

Por defecto `mk` hace \alert{el primer objetivo}.

## Receta para compu (`mkfile`)

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

## Un `mkfile` no tan sencillo

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


## Instalar `mk`

En varias distribuciones linux, es parte de `9base` o `plan9port`:

```
## apt install 9base
```

Se necesita agregar a tu `~/.profile` o `~/.bashrc`:

```
export PATH=$PATH:/usr/lib/plan9/bin
```

## Instalar `mk`

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

# Buenas prácticas

## Buenas prácticas

- Usar [control de versiones](http://nvie.com/posts/a-successful-git-branching-model/ ).

- Mantener los datos y resultados en su directorio
  (y no guardarlos en el mismo control de versiones):

```
mkdir data
printf "/data\n/results\n" > .gitignore
```

- Poner los objetivos virtuales juntos y al inicio:

```
all:V:
        ./targets_5p-3p | xargs mk

lcs:V:
        ./targets_lcs | xargs mk
```

## Buenas prácticas

- En cada objetivo, asegurarse que exista el directorio de los resultados:

```
results/%.lcs:  results/%.sorted
        mkdir -p `dirname $target`
```

- Guardar los resultados en un archivo temporal hasta que haya terminado el proceso correctamente:

```
results/%.lcs:  results/%.sorted
        mkdir -p `dirname $target`
        ./mariana.py $prereq > $target'.build' \
        && mv $target'.build' $target
```

## Buenas prácticas

- Ejecutar mk asignándole el número de procesos que puede ejecutar a la vez:

```
## puedes saber cuántos procesadores tienes usando:
## grep proc /proc/cpuinfo | wc -l
env NPROC=`grep proc /proc/cpuinfo | wc -l` mk
```

- Si tu proceso utiliza múltiples cores (te estoy viendo java ~~):

```
env NPROC=1` mk
```
