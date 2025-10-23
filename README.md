# Guía de Conventional Commits

## 1. ¿Qué es Conventional Commits? 🤔

**Conventional Commits** es un conjunto de reglas simples o una convencion, sobre cómo escribir los mensajes de *commit* en Git.

Su objetivo principal es crear un historial de proyecto explícito y legible, que sea fácil de entender tanto para **humanos** (facilitando la colaboración) como para **máquinas** (permitiendo la automatización).

## 2. Relación con Versioning y ¿qué es Semantic Versioning (SemVer)? 🧩

La principal ventaja de Conventional Commits es su capacidad para automatizar el **Versionamiento Semántico (SemVer)**.

SemVer es un estándar universal para asignar números de versión a un software, diseñado para comunicar el tipo de cambios realizados con cada nueva versión.

El formato es `MAYOR.MENOR.PARCHE` (ej. `1.4.2`):

* **MAYOR (Major):** Se incrementa cuando haces cambios incompatibles con la versión anterior (un *breaking change*).
* **MENOR (Minor):** Se incrementa cuando añades nueva funcionalidad que es compatible con la anterior.
* **PARCHE (Patch):** Se incrementa cuando haces correcciones de errores (*bug fixes*) compatibles con la versión anterior.

Conventional Commits permite que herramientas automáticas lean tu historial de Git y determinen qué número incrementar:
* Un `fix` incrementa `PARCHE`.
* Un `feat` incrementa `MENOR`.
* Un `!` o `BREAKING CHANGE:` en el *footer* incrementa `MAYOR`.

## 3. Especificación de Conventional Commits 🔎



La estructura de un mensaje de commit convencional es la siguiente:
```
<tipo>[optional (<ámbito>)]!: <descripción>

[optional cuerpo]

[optional nota al pie (footer)]
```
* **`<tipo>` (Obligatorio):** El tipo de cambio (ver Sección 4 para la lista completa).
* **`<ámbito>` (Opcional):** El módulo o sección del código afectado (ej. `api`, `auth`, `ui`). Debe ir entre paréntesis.
* **`!` (Opcional):** Un signo de exclamación *antes* de los dos puntos (`:`) indica un cambio que rompe la compatibilidad (un *Breaking Change*).
* **`<descripción>` (Obligatorio):** Resumen corto del cambio, en imperativo y minúsculas (ej. "añadir" en lugar de "añadido").
* **`[cuerpo]` (Opcional):** Una explicación más larga, separada de la descripción por una línea en blanco. Detalla el "qué" y el "porqué" del cambio.
* **`[nota al pie]` (Opcional):** Separado del cuerpo por una línea en blanco. Se usa para referenciar *Issues* (ej. `Closes #123`) o para declarar un `BREAKING CHANGE:`.

> **Importante sobre Breaking Changes:** Un `!` en la cabecera es solo un indicador. *Siempre* se debe detallar el *breaking change* en la **nota al pie**, comenzando con `BREAKING CHANGE: <descripción del cambio>`.



## 4. Tipos y Estructura 🌲

### Tipos de Commit (Type)

Estos son los tipos más comunes definidos por la especificación:

* **feat:** Introduce una nueva funcionalidad (mapea a `MINOR` en SemVer).
* **fix:** Corrige un error o bug (mapea a `PATCH` en SemVer).
* **docs:** Cambios exclusivos en la documentación.
* **style:** Cambios de formato, espacios en blanco, punto y coma, etc. (sin cambio lógico en el código).
* **refactor:** Cambios en el código que no corrigen un error ni añaden funcionalidad (ej. limpiar código).
* **perf:** Un cambio de código que mejora el rendimiento.
* **test:** Añade o modifica tests.
* **build:** Cambios que afectan el sistema de *build* o dependencias externas.
* **ci:** Cambios en los archivos y *scripts* de Integración Continua (CI).
* **chore:** Tareas de mantenimiento que no afectan al código de producción (actualizar dependencias, *scripts*).
* **revert:** Revierte un commit anterior.

### Otras Partes

* **Ámbito (Scope):** (Opcional) Una sección del código afectada por el cambio (ej. `api`, `auth`, `ui`, `login`).
* **Descripción (Subject):** Resumen corto del cambio, escrito en imperativo (ej. "añadir" en lugar de "añadido").
* **Cuerpo (Body):** (Opcional) Una explicación más detallada del cambio, explicando el "qué" y el "porqué".
* **Nota al pie (Footer):** (Opcional) Se usa para información adicional, como referenciar *Issues* (ej. `Closes #123`) o, muy importante, para declarar **`BREAKING CHANGE:`** (un cambio que rompe la compatibilidad).

## 5. Ejemplos 📋

**Ejemplo 1: Un `feat` (MINOR)**
```
feat(auth): add refresh token rotation

Introduce a rotation window and invalidation policy for stolen tokens.
Closes #142
```


**Ejemplo 2: Un `fix` (PATCH)**
```
fix(cart): prevent NPE when promotion price is null

Add guard clause to avoid null dereference in totals calculation.
Refs SHOP-88

```



**Ejemplo 3: Un `refactor` con ruptura (MAJOR)**
```
refactor(api)!: remove deprecated v1 endpoints

Consolidate handlers into v2 controller and simplify DTOs for clarity.

BREAKING CHANGE: /api/v1/orders was removed. Use /api/v2/orders with the new
OrderRequestV2 schema. See MIGRATION.md for mapping and examples.
```


## 6. ¿Por qué usar Conventional Commits? 💻

* **Permite la automatización:** Es la razón principal. Herramientas pueden escanear los *commits* y generar `CHANGELOGs` (un historial de cambios para el usuario) automáticamente.
* **Versionamiento Semántico (SemVer) automático:** Herramientas como `semantic-release` pueden leer los commits (`feat`, `fix`, `BREAKING CHANGE`) para decidir si lanzar una versión `MAJOR`, `MINOR` o `PATCH` sin intervención humana.
* **Un historial de Git limpio y comprensible:** Facilita a cualquier persona (incluido tu "yo" del futuro) entender la evolución del proyecto de un solo vistazo.
* **Facilita la revisión de código (Pull Requests):** Los revisores pueden entender la intención de cada *commit* individual, agilizando el proceso de revisión.
* **Mejora la depuración:** Permite filtrar el historial fácilmente (ej. "mostrarme solo los `feat` y `fix` de la última semana").

## 7. Tips y Herramientas 💡

Recordar la especificación puede ser difícil. Por suerte, puedes forzar su cumplimiento usando herramientas que se integran con Git.

La combinación más popular es **`husky`** + **`commitlint`**:

* **Husky:** Es una herramienta que te permite ejecutar *scripts* fácilmente en los diferentes "ganchos" (hooks) de Git. Un *hook* es un evento, como "antes de hacer commit" (`pre-commit`) o "al recibir el mensaje de commit" (`commit-msg`).
* **Commitlint:** Es un "linter" (un validador de estilo) específico para mensajes de *commit*. Revisa si tu mensaje sigue las reglas de Conventional Commits.

**¿Cómo funcionan juntos?**
Configuras **Husky** para que, cada vez que intentes hacer un *commit*, se active el *hook* `commit-msg`. En ese momento, **Husky** ejecuta **Commitlint**, el cual analiza tu mensaje.

**Resultado:** Si tu mensaje (`git commit -m "..."`) no cumple con el formato (ej. `fix: algo`), **Commitlint** dará un error y Git **rechazará el commit**. Te obligará a corregir el mensaje antes de poder registrar el cambio.

Esto garantiza que *todo* el equipo siga la convención, manteniendo el historial limpio y funcional



## 8. Páginas de referencia 🌎

* [Especificación Oficial de Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/)
* [Documentación de Git (Sitio Oficial)](https://git-scm.com/)
* [Documentación de Git (Docs)](https://git-scm.com/docs)
