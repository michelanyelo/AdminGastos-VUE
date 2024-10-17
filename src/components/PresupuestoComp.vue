<script setup>
import { ref } from 'vue';
import AlertaComp from './AlertaComp.vue';

const presupuesto = ref(0)
const error = ref('')

const definirPresupuesto = () => {
    if (presupuesto.value <= 0) {
        error.value = "Presupuesto no válido"
        setTimeout(() => {
            error.value = ""
        }, 2000)
    } else {
        error.value = ""
    }
}

</script>

<template>
    <form class="presupuesto" @submit.prevent="definirPresupuesto">
        <!-- Alerta slot -->
        <AlertaComp v-if="error.length">
            {{ error }}
        </AlertaComp>
        <div class="campo">
            <label for="nuevo-presupuesto">Definir Presupuesto</label>
            <input v-model.number="presupuesto" min="0" type="number" id="nuevo-presupuesto"
                placeholder="Añade tu presupuesto" class="nuevo-presupuesto">
        </div>
        <input type="submit" value="Definir Presupuesto">
    </form>
</template>

<style scoped>
.presupuesto {
    width: 100%;
}

.campo {
    display: grid;
    gap: 2rem;
}

.presupuesto label {
    font-size: 2.2rem;
    text-align: center;
    color: var(--azul);
    font-weight: 700;
}

.presupuesto input[type="number"] {
    background-color: var(--gris-claro);
    border-radius: 1rem;
    padding: 1rem;
    border: none;
    font-size: 2.2rem;
    text-align: center;
}

.presupuesto input[type="submit"] {
    background-color: var(--azul);
    border: none;
    padding: 1rem;
    text-align: center;
    font-size: 2rem;
    margin-top: 2rem;
    color: var(--blanco);
    font-weight: 900;
    width: 100%;
    transition: background-color 300ms ease;
}

.presupuesto input[type="submit"]:hover {
    background-color: #1048a4;
    cursor: pointer;

}
</style>