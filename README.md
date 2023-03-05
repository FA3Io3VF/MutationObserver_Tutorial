# MutationObserver_Tutorial
JavaScript Mutation Observer Tutorial

---

***MutationObserver*** è la classe di JavaScript che implementa un pattern Observer sugli elementi del DOM di una pagina HTML. 

Il MutationObserver richiede una callback da eeguire a ogni mutazione del DOM, questa riceve un oggetto di tipo MutationRecord che spiega a quale mutazione reagire e come.

Il metodo principale è observe() a cui si da la radice (o sottoradice) del DOM da cui iniziare a monitorare le mutazioni.

## Come funziona in pratica?

***Rilevazione aggiunte a una lista:***

```javascript

const list = document.querySelector('#listID');

const observer = new MutationObserver(function(mutations) {
  mutations.forEach(function(mutation) {
    if (mutation.type === 'childList' && mutation.addedNodes.length > 0) {
      /* E' stato aggiunto un elemento quindi facciamo qualcosa... */
    }
  });    
});

const config = { childList: true }; /* COSA FARA' MAI? */

observer.observe(lista, config);
```

### Il parametro config

E' un paramentro del costruttore di MutationObserver per specificare quali mutazioni gestire e come. 

Questo parametro è una istanza di un oggetto con i seguenti parmentri:

- attributes: booleano di defaut false che dice se osservare le mutazioni degli attributi degli elementi target. 
- attributeFilter: funziona se atributes è true e si tratta di un array di stringhe che specifica, filtra, gli attributi da osservare.
- childList: un booleano che serve a sapere se osservare anche i child dell'elemento del DOM
- characterData: Dice se osservare le mutazioni degli InnerText

_attenzione_ ci sono altri valori.


## Usarlo per rendere dinamica una pagina

### Ecco un esempio utile: aggiorniamo una div con il contenuto dell'altra se cambia

```html
<div id="observed">testo di partenza</div>
<div id="toUpdate">testo che verrà modificato</div>
```
```javascript
const observed = document.querySelector('#observed');
const updated = document.querySelector('#toUpdate');

const observer = new MutationObserver(() => {
  updated.textContent = osserved.textContent;
});

observer.observe(observed, { childList: true });
```

### Osserviamo tutti i nodi testuali di un DOM:

```javascript
const observer = new MutationObserver((mutationsList) => {
  for (const mutation of mutationsList) {
    if (mutation.type === 'characterData') {
      console.log(mutation.target.textContent);
    }
  }
});

observer.observe(document.body, { 
          childList: true,      /* includiamo tutti i nodi figli */
          subtree: true,        /*  includiamo l'intero sottoalbero del DOM */
          characterData: true  /* Incldiamo i nodi testuali */
});
```

La callback controllerà ogni tipo di mutazione ma se muta un testo stamperà il testo con la console.log di ***mutation.target.textContent****.

_Altre Informazioni su:_ htps://developer.mozilla.org/en-US/docs/Web/API/MutationObserver


