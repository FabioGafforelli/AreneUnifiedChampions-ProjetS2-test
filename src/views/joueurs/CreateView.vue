<template>
  <h1 class="font-museomoderno mb-20 mt-10 border-b-2 text-center text-2xl">Inscription d'un joueur</h1>

  <div class="relative mt-12 mb-20 flex flex-col gap-20 px-5">
    <form enctype="multipart/form-data" @submit.prevent="Createjoueurs">
      <div class="grid grid-cols-1 gap-4 lg:flex lg:w-[100%] lg:justify-center">
        <div class="grid place-items-center">
          <img class="preview img-fluid" :src="imageData" />
        </div>
        <div class="grid grid-cols-1 gap-14">
          <div class="flex h-10 overflow-hidden rounded-2xl text-black">
            <div class="flex items-center justify-center border-[1px] bg-blue-700 px-5 text-white">Nom</div>
            <input class="font-museomoderno w-full border-2 bg-white p-5" type="text" placeholder="Nom du joueur" v-model="joueurs.nom" required />
          </div>

          <div class="flex h-12 overflow-hidden rounded-2xl text-black">
            <p class="font-museomoderno border-[1px] bg-blue-700 px-6 py-3 text-white">Adresse Mail</p>
            <input
              class="font-museomoderno w-full border-2 bg-white p-5"
              type="text"
              placeholder="Adresse mail universitaire"
              v-model="joueurs.mail"
              required
            />
          </div>
          <div class="flex h-12 overflow-hidden rounded-2xl text-black">
          <p class="font-museomoderno border-[1px] bg-blue-700 px-6 py-3 text-white">Date naissance</p>
                                <input 
                                    type="date"
                                    class="form-control"
                                    v-model="joueurs.naissance" 
                                    format="dd/mm/yyyy" 
                                    required />                    
                            </div>
            <div class="flex h-12 overflow-hidden rounded-2xl text-black">
                                <p class="font-museomoderno border-[1px] bg-blue-700 px-6 py-3 text-white">
                                  Jeux
                                </p>
                                <select class="custom-select" v-model="joueurs.jeux">
                                    <option selected disabled>S??lectionner un jeu</option>
                                    <option v-for="jeux in Listejeux" :key="jeux.nom">
                                        {{jeux.nom}}
                                    </option>
                                </select>
                            </div>
          <div class="flex h-12 overflow-hidden rounded-2xl text-black">
            <p class="font-museomoderno bg-blue-700 px-6 py-0 text-white">Code discord</p>
            <input
              class="font-museomoderno w-full border-2 bg-white p-5"
              type="text"
              placeholder="Votre code discord (ex: Zerios#9102)"
              v-model="joueurs.discord"
              required
            />
          </div>
          <div class="flex h-10 overflow-hidden rounded-sm text-black">
            <div class="font-museomoderno flex items-center justify-center border-[1px] bg-blue-700 px-5 text-white">Logo</div>
            <div class="custom-file">
              <input type="file" class="custom-file-input" ref="file" id="file" @change="previewImage" />
              <label class="custom-file-label" for="file">S??lectionner l'image</label>
            </div>
          </div>
        </div>
      </div>
      <div class="mt-16 flex w-full justify-center gap-5">
        <button
          class="font-rajdhani mb-20 rounded-2xl bg-blue-600 py-2 px-5 text-white"
          type="submit"
        >
          Ajouter
        </button>
        <button
          class="font-rajdhani mb-20 rounded-2xl bg-blue-600 py-2 px-5 text-white"
          type="button"
        >
          <RouterLink to="/inscription">Annuler</RouterLink>
        </button>
      </div>
    </form>
  </div>
</template>

<script>
import {
  getFirestore, // Obtenir le Firestore
  collection, // Utiliser une collection de documents
  doc, // Obtenir un document par son id
  getDocs, // Obtenir la liste des documents d'une collection
  addDoc, // Ajouter un document ?? une collection
  updateDoc, // Mettre ?? jour un document dans une collection
  deleteDoc, // Supprimer un document d'une collection
  onSnapshot, // Demander une liste de documents d'une collection, en les synchronisant
  query, // Permet d'effectuer des requ??tes sur Firestore
  orderBy, // Permet de demander le tri d'une requ??te query
} from "https://www.gstatic.com/firebasejs/9.8.2/firebase-firestore.js";

// Cloud Storage : import des fonctions
import {
  getStorage, // Obtenir le Cloud Storage
  ref, // Pour cr??er une r??f??rence ?? un fichier ?? uploader
  getDownloadURL, // Permet de r??cup??rer l'adress compl??te d'un fichier du Storage
  uploadString, // Permet d'uploader sur le Cloud Storage une image en Base64
} from "https://www.gstatic.com/firebasejs/9.8.2/firebase-storage.js";

export default {
  name: "CreateView",
  data() {
    return {
      imageData: null, // Image pr??visualis??e
      Listejeux:[],           // Liste des pays pour la nationalit?? du participant
      Listejoueurs: [],
      joueurs: {
        photo: null,
        nom: null, // son pr??nom
        mail: null, // sa photo (nom du fichier)
        naissance: null,
        jeux: null,
        discord: null,
      },
    };
  },
     mounted(){ // Montage de la vue
        // Appel de la liste des pays
        this.getJeux();
    },
    methods : {
        async getJeux(){            
            // Obtenir Firestore
            const firestore = getFirestore();
            // Base de donn??es (collection)  document pays
            const dbJeux = collection(firestore, "jeux");
            // Liste des participants tri??s
            // query permet de faire une requ??te sur Firebase
            // notement pour filtrer, trier ... des donn??es
            //orderBy permet de pr??ciser sur quel ??l??ment trier, et dans quel ordre
            // ici le nom du pays par ordre croissant (asc)            
            const q = query(dbJeux, orderBy('nom', 'asc'));
            // R??cup??ration de la liste des pays ?? partir de la query
            // La liste est synchronis??e
            await onSnapshot(q, (snapshot) => {
                this.Listejeux = snapshot.docs.map(doc => (
                    {id:doc.id, ...doc.data()}
                ))  
console.log("Liste des jeux", this.Listejeux);      
            })      
        },
            mounted(){ // Montage de la vue
        // Appel de la liste des pays
        this.getPays();
    },
    methods : {
        async getPays(){            
            // Obtenir Firestore
            const firestore = getFirestore();
            // Base de donn??es (collection)  document pays
            const dbPays = collection(firestore, "pays");
            // Liste des participants tri??s
            // query permet de faire une requ??te sur Firebase
            // notement pour filtrer, trier ... des donn??es
            //orderBy permet de pr??ciser sur quel ??l??ment trier, et dans quel ordre
            // ici le nom du pays par ordre croissant (asc)            
            const q = query(dbPays, orderBy('nom', 'asc'));
            // R??cup??ration de la liste des pays ?? partir de la query
            // La liste est synchronis??e
            await onSnapshot(q, (snapshot) => {
                this.listePays = snapshot.docs.map(doc => (
                    {id:doc.id, ...doc.data()}
                ))  
console.log("Liste des pays", this.listePays);      
            })      
        },
    },

    previewImage: function (event) {
      this.file = this.$refs.file.files[0];

      this.joueurs.photo = this.file.name;
      // Reference to the DOM input element
      // Reference du fichier ?? pr??visualiser
      var input = event.target;
      // On s'assure que l'on a au moins un fichier ?? lire
      if (input.files && input.files[0]) {
        // Creation d'un filereader
        // Pour lire l'image et la convertir en base 64
        var reader = new FileReader();
        // fonction callback appell??e lors que le fichier a ??t?? charg??
        reader.onload = (e) => {
          // Read image as base64 and set to imageData
          // lecture du fichier pour mettre ?? jour
          // la pr??visualisation
          this.imageData = e.target.result;
        };
        // Demarrage du reader pour la transformer en data URL (format base 64)
        reader.readAsDataURL(input.files[0]);
      }
    },

    async Createjoueurs() {
      // Obtenir storage Firebase
      const storage = getStorage();
      // R??f??rence de l'image ?? uploader
      const refStorage = ref(storage, "photojoueurs/" + this.joueurs.photo);
      // Upload de l'image sur le Cloud Storage
      await uploadString(refStorage, this.imageData, "data_url").then((snapshot) => {
        console.log("Uploaded a base64 string");

        const db = getFirestore();
        const docRef = addDoc(collection(db, "joueurs"), this.joueurs);
      });

      this.$router.push("/inscription");
    },
},
}
</script>
