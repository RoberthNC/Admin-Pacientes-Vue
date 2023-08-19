<script setup>
  import { ref, reactive } from "vue";

  import { uid } from "uid";

  import Header from "./components/Header.vue";
  import Formulario from "./components/Formulario.vue";
  import Mascota from "./components/Mascota.vue";

  const mascotas = ref([]);

  const mascota = reactive({
        id:null,
        nombre:"",
        propietario:"",
        email:"",
        alta:"",
        sintomas:""
  });

  const guardarMascota = () => {
    mascotas.value.push({
      ...mascota,
      id:uid()
    });
    mascota.nombre = "";
    mascota.propietario = "";
    mascota.email = "";
    mascota.alta = "";
    mascota.sintomas = "";
  }
  
</script>

<template>
  <div class="container mx-auto mt-20">
    <Header />
    <div class="mt-12 md:flex">
      <Formulario
        v-model:nombre="mascota.nombre"
        v-model:propietario="mascota.propietario"
        v-model:email="mascota.email"
        v-model:alta="mascota.alta"
        v-model:sintomas="mascota.sintomas"
        @guardar-mascota="guardarMascota"
      />
      <div class="md:w-1/2 md:h-screen overflow-y-scroll">
        <h3 class="font-black text-3xl text-center">Administra tus Pacientes</h3>

        <div v-if="mascotas.length > 0">
          <p class="text-lg mt-5 text-center mb-10">
            Información de
            <span class="text-indigo-600 font-bold">Pacientes</span>
          </p>
          <Mascota
            v-for="mascota in mascotas"
            :mascota="mascota"
          />
        </div>
        <p v-else class="mt-20 text-2xl text-center">No hay pacientes</p>
      </div>
    </div>
  </div>
</template>

<style>

</style>