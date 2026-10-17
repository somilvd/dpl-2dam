---
layout: post
title: "Introducción a la programación orientada a objetos en Java"
date: 2026-10-13 22:00:00 +0100
categories: java programacion
---

## ¿Qué es la programación orientada a objetos?

La programación orientada a objetos (POO) es una forma de desarrollar programas organizando el código mediante **clases y objetos**. Java es uno de los lenguajes que utiliza este paradigma.

## Clases y objetos

Una clase define las características y los comportamientos que tendrán sus objetos. Por ejemplo, podemos crear una clase `Coche` con atributos como la marca y el modelo.

```java
public class Coche {
    String marca;
    String modelo;

    public void mostrarInformacion() {
        System.out.println(marca + " " + modelo);
    }
}
```

Después podemos crear un objeto a partir de esa clase:

```java
Coche coche = new Coche();
coche.marca = "Toyota";
coche.modelo = "Corolla";
coche.mostrarInformacion();
```

El programa mostraría:

```text
Toyota Corolla
```

## ¿Por qué es útil la POO?

- **Organización:** permite estructurar el código en clases.
- **Reutilización:** podemos crear varios objetos a partir de una misma clase.
- **Mantenimiento:** facilita modificar y ampliar los programas.

## Conclusión

La programación orientada a objetos es una base importante para aprender Java y desarrollar aplicaciones más organizadas. A medida que avance en mis estudios, iré publicando más ejemplos y conceptos relacionados con este lenguaje.
