<template>
  <!-- <div class="home"></div> -->
  <AddPet @add-pet="addPet"  />
  <Pets @remove-pet="removePet" @add-favorite="addFavorite" :pets="pets" />
</template>

<script>
import Pets from "../components/Pets"
import AddPet from "../components/AddPet"

export default {
  name: 'HomeView',
  components: { Pets, AddPet },
  methods: {
    // addPet(pet){
    //   this.pets = [...this.pets, pet];
    // },
    async addPet(pet) {
      const res = await fetch("https://63f36704fe3b595e2ee11976.mockapi.io/pets", {
        method: "POST",
        headers: {
          "Content-type": "application/json"
        },
        body: JSON.stringify(pet)
      })

      const data = await res.json();
      this.pets = [...this.pets, data]
    },
    // removePet(id){
    //   // console.log("Home", id);
    //   if(confirm("Er du Sikker på du vil fjerne dette kæledyr?")){
    //     this.pets = this.pets.filter((pet) => pet.id !== id)
    //   }  
    // },
    async removePet(id) {
      if (confirm("Are you sure you want to remove this pet?")) {
        const res = await fetch(
          `https://63f36704fe3b595e2ee11976.mockapi.io/pets/${id}`,
          {
            method: "DELETE",
          }
        );

        res.status === 200
          ? (this.pets = this.pets.filter((pet) => pet.id !== id))
          : alert("Delete failed!");
      }
    },
    async addFavorite(id) {
      const addFavorite = await this.fetchPet(id);
      const updatedFavorite = {
        ...addFavorite,
        isFavorite: !addFavorite.isFavorite,
      };
      const res = await fetch(
        `https://63f36704fe3b595e2ee11976.mockapi.io/pets/${id}`,
        {
          method: "PUT",
          headers: {
            "Content-type": "application/json",
          },
          body: JSON.stringify(updatedFavorite),
        }
      );
      const data = await res.json();

      this.pets = this.pets.map((pet) =>
        pet.id === id ? { ...pet, isFavorite: data.isFavorite } : pet
      );
    },
    async fetchPets(){
      const res = await fetch("https://63f36704fe3b595e2ee11976.mockapi.io/pets")
      const data = await res.json();
      return data
    },
    async fetchPet(id){
      const res = await fetch(`https://63f36704fe3b595e2ee11976.mockapi.io/pets/${id}`)
      const data = await res.json();
      return data
    }

  
  },
  data() {
    return {
      pets: [],
    };
  },
  async created() {
    this.pets = await this.fetchPets();
    // this.pets = [
    //   {
    //     id: 1,
    //     name: "Ruby",
    //     age: 2,
    //     url:
    //       "https://images.dog.ceo/breeds/terrier-american/n02093428_4552.jpg",
    //     isFavorite: true,
    //   },
    //   {
    //     id: 2,
    //     name: "Coco",
    //     age: 4,
    //     url: "https://images.dog.ceo/breeds/pointer-german/n02100236_3025.jpg",
    //     isFavorite: false,
    //   },
    //   {
    //     id: 3,
    //     name: "HIHI",
    //     age: 99,
    //     url: "https://images.dog.ceo/breeds/pointer-german/n02100236_3025.jpg",
    //     isFavorite: false,
    //   }
    // ];

  }
}
</script>