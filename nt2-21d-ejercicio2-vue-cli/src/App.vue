<script setup>
import { ref, computed } from 'vue'

const filtroNombre = ref('')
const filtroDni = ref('')

const personas = ref([
  { nombre: 'Daniel', apellido: 'Sanchez', correo: 'danielsanchez68@hotmail.com', dni: '20442873' },
  { nombre: 'Federico', apellido: 'Cotler', correo: 'federicotler@.gmail.com', dni: '41836255' },
  { nombre: 'Ana', apellido: 'Suarez', correo: 'anasuarez@.gmail.com', dni: '87654321' },
  { nombre: 'Lionel', apellido: 'Messi', correo: 'leomessi@gmail.com', dni: '10101010' },
])

const personasFiltradas = computed(() => {
  if (!filtroNombre.value && !filtroDni.value) {
    return personas.value
  }

  return personas.value.filter(persona => {
    const nombreCompleto = `${persona.nombre} ${persona.apellido}`.toLowerCase()
    const dni = persona.dni.toLowerCase()

    let cumpleNombre = true
    let cumpleDni = true

    if (filtroNombre.value.length >= 3) {
      cumpleNombre = nombreCompleto.includes(filtroNombre.value.toLowerCase())
    }
    if (filtroDni.value.length >= 3) {
      cumpleDni = dni.includes(filtroDni.value.toLowerCase())
    }

    return cumpleNombre && cumpleDni
  })
})

const mostrarAdvertencia = computed(() => {
  return (
    (filtroNombre.value.length > 0 && filtroNombre.value.length < 3) ||
    (filtroDni.value.length > 0 && filtroDni.value.length < 3)
  )
})
</script>

<template>
  <div class="container mt-4">
    <h2>Listado de Personas</h2>

    <div class="mb-3">
      <label for="filtroNombre" class="form-label">Filtro Nombre / Apellido</label>
      <input
        id="filtroNombre"
        type="text"
        class="form-control"
        v-model="filtroNombre"
        placeholder="Ingrese al menos 3 caracteres"
      />
    </div>

    <div class="mb-3">
      <label for="filtroDni" class="form-label">Filtro DNI</label>
      <input
        id="filtroDni"
        type="text"
        class="form-control"
        v-model="filtroDni"
        placeholder="Ingrese al menos 3 caracteres"
      />
    </div>

    <div v-if="mostrarAdvertencia" class="alert alert-warning" role="alert">
      Por favor ingrese al menos 3 caracteres en los filtros para que se aplique la búsqueda.
    </div>

    <div class="row">
      <div class="col-md-4" v-for="persona in personasFiltradas" :key="persona.dni">
        <div class="card mb-3">
          <div class="card-body">
            <h5 class="card-title">{{ persona.nombre }} {{ persona.apellido }}</h5>
            <p class="card-text">DNI: {{ persona.dni }}</p>
            <p class="card-text">Correo: {{ persona.correo }}</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
