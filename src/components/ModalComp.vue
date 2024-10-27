<script setup>
import { computed, ref } from 'vue'
import cerrarModal from "../assets/img/cerrar.svg"
import AlertaComp from "./AlertaComp.vue"

const error = ref('')
const emit = defineEmits(["cerrar-modal", "guardar-gasto", "eliminar-gasto", "update:nombre", "update:cantidad", "update:categoria"])
const props = defineProps({
    modal: {
        type: Object,
        required: true
    },
    nombre: {
        type: String,
        required: true
    },
    cantidad: {
        type: [String, Number],
        required: true
    },
    categoria: {
        type: String,
        required: true
    },
    disponible: {
        type: Number,
        required: true
    },
    id: {
        type: [String, null],
        required: true
    }
})

const old_gasto = props.cantidad

const mostrarError = (mensaje) => {
    error.value = mensaje
    setTimeout(() => {
        error.value = ''
    }, 3000)
}

const agregarGasto = () => {
    const { nombre, cantidad, categoria, disponible, id } = props

    if ([nombre, cantidad, categoria].includes('')) {
        return mostrarError('Todos los campos son obligatorios')
    }

    if (cantidad <= 0) {
        return mostrarError('Cantidad debe ser superior a 0')
    }

    const presupuestoPermitido = id ? old_gasto + disponible : disponible

    if (cantidad > presupuestoPermitido) {
        return mostrarError('Has excedido el presupuesto disponible')
    }

    emit('guardar-gasto')
}

const isEditing = computed(() => {
    return props.id
})
</script>


<template>
    <div class="modal">
        <div class="cerrar-modal">
            <img :src="cerrarModal" alt="Cerrar Modal" @click="$emit('cerrar-modal')">
        </div>

        <div class="contenedor contenedor-formulario" :class="modal.animar ? 'animar' : 'cerrar'">
            <form class="nuevo-gasto" @submit.prevent="agregarGasto">
                <legend>{{ isEditing ? 'Editar Gasto' : 'Agregar Gasto' }}</legend>

                <AlertaComp v-if="error">{{ error }}</AlertaComp>

                <div class="campo">
                    <label for="nombre">Nombre Gasto:</label>
                    <input type="text" id="nombre" placeholder="Añade el nombre del gasto" :value="nombre"
                        @input="$emit('update:nombre', $event.target.value)">
                </div>

                <div class="campo">
                    <label for="cantidad">Cantidad:</label>
                    <input type="text" id="cantidad" placeholder="Añade la cantidad del gasto, ej. 3000"
                        :value="cantidad" @input="$emit('update:cantidad', +$event.target.value)">
                </div>

                <div class="campo">
                    <label for="categoria">Categoria:</label>
                    <select id="categoria" :value="categoria" @input="$emit('update:categoria', $event.target.value)">
                        <option value="">-- Selecione --</option>
                        <option value="ahorro">Ahorro</option>
                        <option value="comida">Comida</option>
                        <option value="casa">Casa</option>
                        <option value="gastos">Gastos Varios</option>
                        <option value="ocio">Ocio</option>
                        <option value="salud">Salud</option>
                        <option value="suscripciones">Suscripciones</option>
                    </select>
                </div>

                <input type="submit" :value="[isEditing ? 'Editar Gasto' : 'Agregar Gasto']">
            </form>

            <button type="button" class="btn-eliminar" v-if="isEditing" @click="$emit('eliminar-gasto')">Eliminar
                Gasto</button>

        </div>
    </div>
</template>

<style scoped>
.modal {
    position: absolute;
    background-color: rgb(0 0 0 / 0.9);
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
}

.cerrar-modal {
    position: absolute;
    right: 3rem;
    top: 3rem;
}

.cerrar-modal img {
    width: 3rem;
    cursor: pointer;
}

.contenedor-formulario {
    transition: all 300ms ease-in;
    opacity: 0;
    transform: scale(0.9);
    visibility: hidden;
    padding: 6rem;
}

.contenedor-formulario.animar {
    opacity: 1;
    transform: scale(1);
    visibility: visible;
}

.contenedor-formulario.cerrar {
    opacity: 0;
    transform: scale(0.9);
    visibility: hidden;
}

.nuevo-gasto {
    margin: 10rem auto 0 auto;
    display: grid;
    gap: 2rem;
}

.nuevo-gasto legend {
    text-align: center;
    color: var(--blanco);
    font-size: 3rem;
    font-weight: 700;
}

.campo {
    display: grid;
    gap: 2rem;
}

.nuevo-gasto input,
.nuevo-gasto select {
    background-color: var(--gris-claro);
    border-radius: 1rem;
    padding: 1rem;
    border: none;
    font-size: 2.2rem;
}

.nuevo-gasto label {
    color: var(--blanco);
    font-size: 3rem;
}

.nuevo-gasto input[type="submit"] {
    background-color: var(--azul);
    color: var(--blanco);
    font-weight: 700;
    cursor: pointer;
}

.btn-eliminar {
    padding: 1rem;
    width: 100%;
    background-color: #ef4444;
    font-size: 1.2rem;
    color: var(--blanco);
    margin-top: 10rem;
    cursor: pointer;
    border: none;
}
</style>