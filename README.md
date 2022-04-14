### Crud Svelte

Crud realizado con el Framework Front-end SvelteKit para mostrar las vistas e interactuar con la aplicacion.
El backend fue realizado con el servicio Supabase que permite crear un backend con una base de datos Postgres.

##Instalación

## Paso 1

**Opcion 1 - Descargar Zip:** Dar click en el boton code, luego en Download ZIP y una vez descargado descomprimir el archivo.

**Opcion 2 - Clonar repositorio:** Dar click en el boton code y el su terminal escribir el siguiente comando.

    git clone https://github.com/Alan13377/Prueba-Crud.git

Para acceder a la carpeta en la terminal y abrir el proyecto, ejecutar los siguientes comandos. (Es necesario tener visual studio code instalado)

    cd .\Prueba-Crud\
    code .

##Paso 2
Una vez abierto el proyecto en el editor visual studio,
ejecutar en la terminal el siguente comando.

    npm i

o

    npm install

Con el comando npm install instalaremos las dependencias necesarias para ejecutar el proyecto.

##Paso 3

En la raiz del proyecto crear un archivo .env en donde colocaremos las siguientes lineas

    VITE_SUPABASE_URL=https://ivczbinubrzuwcdoemzl.supabase.co
    VITE_SUPABASE_ANON_KEY=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6Iml2Y3piaW51YnJ6dXdjZG9lbXpsIiwicm9sZSI6ImFub24iLCJpYXQiOjE2NDk4MDUxMjQsImV4cCI6MTk2NTM4MTEyNH0.lltsJ-wrGUjhvi3vHvJCeHCXU6_RRI1AgWvxbVpFq0U

Guardar los cambios

##Paso 4
En la termina ejecutar el siguiente comando.

    npm run dev

Para poder ejecutar el proyecto.

##Paso 5
Una vez ejecutado el comando npm run dev
acceder a la direccion http://localhost:3000
para poder ver el proyecto.

## Link del Proyecto

Puede ver el proyecto en funcionamiento en el siguiente enlace.
https://crud-productos.netlify.app/

# create-svelte

Everything you need to build a Svelte project, powered by [`create-svelte`](https://github.com/sveltejs/kit/tree/master/packages/create-svelte).

## Creating a project

If you're seeing this, you've probably already done this step. Congrats!

```bash
# create a new project in the current directory
npm init svelte@next

# create a new project in my-app
npm init svelte@next my-app
```

> Note: the `@next` is temporary

## Developing

Once you've created a project and installed dependencies with `npm install` (or `pnpm install` or `yarn`), start a development server:

```bash
npm run dev

# or start the server and open the app in a new browser tab
npm run dev -- --open
```

## Building

To create a production version of your app:

```bash
npm run build
```

You can preview the production build with `npm run preview`.

> To deploy your app, you may need to install an [adapter](https://kit.svelte.dev/docs/adapters) for your target environment.
