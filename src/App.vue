<script setup>
    import { ref, reactive } from 'vue'
    import { uid } from 'uid';
    import Header from './components/Header.vue';
    import Formulario from './components/Formulario.vue';
    import Paciente from './components/Paciente.vue';

    const pacientes = ref([])

    const paciente = reactive({
        id: null,
        nombre: '',
        propietario: '',
        email: '',
        alta: '',
        sintomas: ''
    })

    const guardarPaciente = () => {
      if(paciente.id) {

          const {id} = paciente
          const i = pacientes.value.findIndex((pacienteState) => pacienteState === id)
          pacientes.value[i] = {...paciente}
          console.log('Edicion', id, i)
      } else {
          console.log('Registro Nuevo')
          pacientes.value.push({
          ...paciente,id: uid()})
      }
      
      //Reiniciar el objeto
        paciente.nombre = ''
        paciente.propietario = ''
        paciente.email = ''
        paciente.alta = ''
        paciente.sintomas = ''
        paciente.id = null
    }

    const actualizarPaciente = (id) => {
      console.log('actualizando...', id)     
      const pacienteEditar = pacientes.value.filter( paciente => paciente.id === id)[0]
      Object.assign(paciente, pacienteEditar)
    }

</script>

<template>
  <div class="container mx-auto mt-20">
      <Header/>  

      <div class="mt-12 md:flex">
          <Formulario
          v-model:nombre="paciente.nombre"
          v-model:propietario="paciente.propietario"
          v-model:email="paciente.email"
          v-model:alta="paciente.alta"
          v-model:sintomas="paciente.sintomas"
          @guardar-paciente="guardarPaciente"
          :id="paciente.id"
          />

          <div class="md:w-1/2 md:h-screen overflow-y-scroll">
            <h3 class="font-black text-3xl text-center">Administrador de Pacientes</h3>

            <div v-if="pacientes.length > 0">
              <p class="text-lg mt-5 text-center mb-10">
                Información de
                <span class="text-indigo-600 font-bold">Pacientes</span>
              </p>
                <Paciente
                    v-for="paciente in pacientes"
                      :paciente="paciente"
                      @actualizar-paciente="actualizarPaciente"
                />
            </div>
            <p v-else class="mt-20 text-2xl text-center">No Hay Pacientes</p>
          </div>
      </div>
  </div>
</template>


