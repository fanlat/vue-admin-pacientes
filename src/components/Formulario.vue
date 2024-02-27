<script setup>
 import { reactive, computed } from 'vue';
 import Alerta from './Alerta.vue';
//  const nombre = ref('');

//  se crea la funcíon que llena la  ref nombre con el valor del input
//  const leerNombre = (e) => {
//      nombre.value = e.target.value;
//  }

// se definen los emits para pasar los datos al padre

const emit = defineEmits(['update:nombre', 'update:propietario', 'update:email', 'update:alta', 'update:sintomas', 'guardar-paciente']);

// para los poder ver los valores devemos definir los props

const props = defineProps({
    id: {
        type: [String, null],
        required: false
    },
    nombre: {
        type: String,
        required: true
    },
    propietario: {
        type: String,
        required: true
    },
    email: {
        type: String,
        required: true
    },
    alta: {
        type: String,
        required: true
    },
    sintomas: {
        type: String,
        required: true
    }
});

const alerta = reactive({
    tipo: '',
    mensaje: ''
});

// se pasa ala app.vue para mejor manejo de los datos
// const paciente = reactive({
//      nombre: '',
//      propietario: '',
//      email: '',
//      alta: '',
//      sintomas: ''
//  });

 const validando = () => {

    // object.value devuelve un array con los valores de las propiedades de un objeto
    // el .includes busca si el array tiene un valor vacio
     if(Object.values(props).includes('')){
         alerta.mensaje ='Todos los campos son obligatorios';
         alerta.tipo = 'error';
         return;
     }

     emit('guardar-paciente', props);
     alerta.mensaje = 'Paciente registrado correctamente';
     alerta.tipo = 'exito';

     setTimeout(() => {
         alerta.mensaje = '';
         alerta.tipo = '';
     }, 3000);
 }

 // se crea una variable computada para saber si se esta editando o no
 // el computed es una forma de reactividad que se actualiza cuando cambia el valor de la variable
 const editando = computed(() => {
     return props.id;
 });

</script>

<template>
    <div class="md:w-1/2">
        <h2 class=" font-black text-3xl text-center">Seguimiento Pacientes</h2>
        <p class="text-lg mt-5 text-center mb-10">
            Añade Pacientes y
            <span class="text-indigo-600 font-bold">Adminístralos</span>
        </p> 
        <Alerta
            v-if="alerta.mensaje"
            :alerta="alerta"
        />
        <!-- el event modifiers .prevent del @submit reemplaza al prevenrDefault() de javascript -->
        <form
            class=" bg-white shadow-md rounded-lg py-10 px-5 mb-10"
            @submit.prevent="validando"
        >
            <div class="mb-5">
                <label for="mascota" class="block text-gray-700 uppercase font-bold">
                    Nombre Mascota
                </label>
                <!-- en esta opción se se coloca directo en el input la función que llena el nombre -->
                <!-- <input 
                    type="text" 
                    id="mascota" 
                    placeholder="Nombre de la mascota" 
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                    :value="nombre"
                    @input="(e)=> nombre = e.target.value"
                > -->
                <!-- en este caso se llama a la función que llena el nombre leerNombre -->
                <!-- <input 
                    type="text" 
                    id="mascota" 
                    placeholder="Nombre de la mascota" 
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                    :value="nombre"
                    @input="leerNombre"
                > -->
                <!-- v-model es una directiva de vue que hace lo mismo que el ejemplo anterior pero de un modo
                     mucho mas resumido y en una sola linea -->
                     <!-- usamos @input para indicar cual es el emit que pasara el valor, y a su vez se le pasa el valor con $event.target.value -->
                <input 
                    type="text" 
                    id="mascota" 
                    placeholder="Nombre de la mascota" 
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                    :value="nombre"
                    @input="$emit('update:nombre', $event.target.value)"
                >

            </div>
            <div class="mb-5">
                <label for="propietario" class="block text-gray-700 uppercase font-bold">
                    Nombre Propietario
                </label>
                <input 
                    type="text" 
                    id="propietario" 
                    placeholder="Nombre de o la propietario/a" 
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                    :value="propietario"
                    @input="$emit('update:propietario', $event.target.value)"
                >
                
            </div>
            <div class="mb-5">
                <label for="propietario" class="block text-gray-700 uppercase font-bold">
                    email
                </label>
                <input 
                    type="email" 
                    id="email" 
                    placeholder="Email de o la propietario/a" 
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                    :value="email"
                    @input="$emit('update:email', $event.target.value)"
                >
                
            </div>
            <div class="mb-5">
                <label for="alta" class="block text-gray-700 uppercase font-bold">
                    Alta
                </label>
                <input 
                    type="date" 
                    id="alta" 
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                    :value="alta"
                    @input="$emit('update:alta', $event.target.value)"
                >
                
            </div>
            <div class="mb-5">
                <label for="sintomas" class="block text-gray-700 uppercase font-bold">
                    Síntomas
                </label>
                <textarea 
                    id="sintomas" 
                    placeholder="Describe los síntomas" 
                    class="h-40 border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                    :value="sintomas"
                    @input="$emit('update:sintomas', $event.target.value)"
                />
                
            </div>
            <input 
                type="submit" 
                class="bg-indigo-600 w-full text-white uppercase font-bold hover:bg-indigo-500 cursor-pointer transition-colors p-3" 
                :value="[editando ? 'Guardar cambios' : 'Registrar Paciente']"
            >
            <!-- los corchetes sirven para indicar que se usara un ternario -->
        </form>
    </div>
</template>