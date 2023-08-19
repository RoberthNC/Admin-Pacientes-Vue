<script setup>
  import { ref, reactive, watch, onMounted } from "vue";

  import { uid } from "uid";

  import Header from "./components/Header.vue";
  import Formulario from "./components/Formulario.vue";
  import Mascota from "./components/Mascota.vue";

  const mascotas = ref([]);

  onMounted(() => {
    mascotas.value = JSON.parse(localStorage.getItem("mascotas"))??[];
  });

  const mascota = reactive({
        id:null,
        nombre:"",
        propietario:"",
        email:"",
        alta:"",
        sintomas:""
  });

  watch(mascotas, ()=>{
    guardarLocalStorage();
  },{
    deep:true
  });

  const guardarLocalStorage = () => {
    localStorage.setItem("mascotas", JSON.stringify(mascotas.value));
  }

  const guardarMascota = () => {
    if(mascota.id){
      const { id } = mascota;
      const i = mascotas.value.findIndex(mascotaState => mascotaState.id === id);
      mascotas.value[i] = {...mascota};
    }else{
      mascotas.value.push({
        ...mascota,
        id:uid()
      });
    }

    Object.assign(mascota,{
      id:null,
      nombre:"",
      propietario:"",
      email:"",
      alta:"",
      sintomas:"",
    });
  }

  const actualizarMascota = id => {
    const mascotaEditar = mascotas.value.filter(mascotaState => mascotaState.id === id)[0];
    Object.assign(mascota, mascotaEditar);
  }

  const eliminarMascota = id => {
    mascotas.value = mascotas.value.filter(mascotaState => mascotaState.id !== id);
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
        :id="mascota.id"
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
            @actualizar-mascota="actualizarMascota"
            @eliminar-mascota="eliminarMascota"
          />
        </div>
        <p v-else class="mt-20 text-2xl text-center">No hay pacientes</p>
      </div>
    </div>
  </div>
</template>

<style>

</style>