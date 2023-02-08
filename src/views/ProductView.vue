<template>
    <div>
<table>
    <caption>Liste des produits</caption>
    <tr>
        <th>Nom</th>
        <th>Prix</th>
        <th>Stock</th>
        <th>Commandés</th>
    </tr>
    <!-- Si le tableau des catégories n'est pas vide -->
    <tr v-for="produit in data.listeProduit" :key="produit.reference">
        <td>{{ produit.nom }}</td>
        <td>{{ produit.prixUnitaire }}</td>
        <td>{{ produit.unitesEnStock }}</td>
        <td>{{ produit.unitesCommandees }}</td>
        
    </tr>
    <tr>
        <td><button  @click="PagePrincipale(data.links.first.href)">⇇</button></td>
        <td><button @click="PagePrecedente(data.links.prev.href)">←</button></td>
        <td><button @click="PageSuivante(data.links.next.href)">→</button></td>
        <td><button  @click="PageDerniere(data.links.last.href)">⇉</button></td>
    </tr>
</table>
</div>
</template>



<script setup>
import { reactive, onMounted } from "vue";
import { BACKEND, doAjaxRequest } from "../api";

let data = reactive({
    listeProduit: [],
    links:[]
});

function chargeProduit() {
    // Appel à l'API pour avoir la liste des catégories
    // Trié par code, descendant
    // Verbe HTTP GET par défaut
    doAjaxRequest(BACKEND + "/api/produits?page=1&size=5")
        .then((json) => {
            data.listeProduit = json._embedded.produits;
            data.links = json._links
            console.log(data.listeProduit)
        })
        .catch((error) => alert(error.message));
}

onMounted(chargeProduit);

function PageSuivante(suivant){
    doAjaxRequest(suivant)
        .then((json) => {
            data.listeProduit = json._embedded.produits;
            data.links = json._links
        })
        .catch((error) => alert(error.message));
}
function PagePrecedente(precedent){
    doAjaxRequest(precedent)
        .then((json) => {
            data.listeProduit = json._embedded.produits;
            data.links = json._links

        })
        .catch((error) => alert(error.message));
}
// last modification
function PagePrincipale(first){
    doAjaxRequest(first)
        .then((json) => {
            data.listeProduit = json._embedded.produits;
            data.links = json._links

        })
        .catch((error) => alert(error.message));
}
// dernière page
function PageDerniere(last){
    doAjaxRequest(last)
        .then((json) => {
            data.listeProduit = json._embedded.produits;
            data.links = json._links

        })
        .catch((error) => alert(error.message));
}



</script>

<style scoped>
td,
th {
    border: 1px solid #ddd;
    padding: 8px;
}


th {
    padding-top: 12px;
    padding-bottom: 12px;
    text-align: left;
    background-color: #232623;
    color: rgb(255, 255, 255);
}
</style>