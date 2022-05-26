# Vue CLI

## Creazione di un nuovo progetto #0

1. Installiamo vue a livello globale: <br>
``` npm install -g @vue/cli ```

2. Verifichiamo che l'installazione sia andata a buon fine: <br>
``` vue --version ```

### Se dovessi riscontrare qualche problema con la PowerShell di Windows, lancia questo comando

``` Set-ExecutionPolicy remotesigned ```

Poi chiudi e riapri la PowerShell ed eventualmente
anche Visual Studio Code, se hai aperto il terminale.

## Creazione di un nuovo progetto #1

1. Creiamo un nuovo progetto: <br>
``` vue create nome-progetto ```

2. Scegliamo Manually select features e premiamo Enter: <br>
```
    ? Please pick a preset:
      Default ([Vue 2], babel, eslint)
      Default (Vue 3 Preview) ([Vue 3], babel, eslint)
    > Manually select features
```
## Creazione di un nuovo progetto #2

Selezioniamo con la barra spaziatrice CSS Pre-Processor
Poi premiamo Enter.

Devono essere selezionate: 
1. Babel
2. CSS Pre-processors
3. Linter / Formatter
4. Choose Vue Version

## Creazione di un nuovo progetto #3

Selezioniamo la versione 2.x

## Creazione di un nuovo progetto #4

Selezioniamo Dart-Sass e premiamo Enter

## Creazione di un nuovo progetto #5

Se lo desideriamo possiamo usare un Linter e relativo Prettier

``` ESLint with error prevention only ``` ||  ```ESLint + Prettier```
<br>(o uno a l'altro)

## Creazione di un nuovo progetto #6

1. Infine possiamo salvare questo preset
2. Attendiamo che il progetto sia generato

## Creazione di un nuovo progetto #7

Facciamo: <br>
```cd nome-progetto```
<br>e poi: <br>
```npm run serve```

## Puliamo la cartella progetto dai file esempio di vue:

1. logo.png in src/assets
2. HelloWorld.vue in src/components
3. in App.vue dobbiamo eliminare l'import di HelloWorld, il nome nei components dello script e il tag HelloWorld nel template.
4. favicon.ico in public

# Organizzazione file .vue

```vue
<template>
  <div>
    //qui va l'html
    //<NewComponent/>
  </div>
</template>

<script>
//per importare altri componenti
//import NewComponent from 'path';

  // qui va il codice javascript
  export default {
    components: {
      //NewComponent
    }
  }
</script>

<style lang="scss" scoped>
  //qui il CSS
</style>
```




