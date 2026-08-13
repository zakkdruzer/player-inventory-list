<template>
  <div class="container py-4">
    <div class="row justify-content-center">
      <div class="col-md-10 col-lg-8">
        <div class="card inventory-card">
          <div class="card-header text-center">
            <h1 class="inventory-title">Player Inventory List</h1>
            <p class="inventory-subtitle">
              Gestiona los items de tu personaje
            </p>
          </div>

          <div class="card-body">
            <!-- Formulario para agregar items -->
            <form @submit.prevent="agregarItem" class="mb-4">
              <div class="row g-2">
                <div class="col-md-4">
                  <input v-model="nuevoNombre" type="text" class="form-control inventory-input"
                    placeholder="Nombre del item" />
                </div>
                <div class="col-md-2">
                  <input v-model.number="nuevaCantidad" type="number" class="form-control inventory-input"
                    placeholder="Cant." min="1" />
                </div>
                <div class="col-md-2">
                  <input v-model.number="nuevoValor" type="number" class="form-control inventory-input"
                    placeholder="Valor" min="0" />
                </div>
                <div class="col-md-4">
                  <button type="submit" class="btn btn-primary w-100 inventory-btn">
                    Agregar item
                  </button>
                </div>
              </div>

              <!-- Mensajes de error -->
              <div v-if="errorFormulario" class="alert inventory-alert inventory-alert-warning mt-2" role="alert">
                {{ errorFormulario }}
              </div>
            </form>

            <!-- Estado vacío -->
            <div v-if="items.length === 0" class="text-center inventory-empty">
              <p class="inventory-status">
                Inventario vacío. Agrega tu primer item.
              </p>
            </div>

            <!-- Lista de items -->
            <div v-else>
              <div class="table-responsive">
                <table class="table table-dark inventory-table">
                  <thead>
                    <tr>
                      <th>Item</th>
                      <th>Cantidad</th>
                      <th>Valor unit.</th>
                      <th>Subtotal</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr v-for="item in items" :key="item.id" class="inventory-row"
                      :class="{ 'item-adquirido': item.adquirido }">
                      <td>
                        <div class="form-check">
                          <input class="form-check-input" type="checkbox" :id="'adquirido-' + item.id"
                            v-model="item.adquirido" />
                          <label class="form-check-label" :for="'adquirido-' + item.id">
                            {{ item.nombre }}
                          </label>
                        </div>
                      </td>
                      <td>{{ item.cantidad }}</td>
                      <td>{{ item.valor }}</td>
                      <td>{{ item.cantidad * item.valor }}</td>
                      <td>
                        <button class="btn btn-sm btn-danger inventory-btn-delete" @click="eliminarItem(item.id)">
                          Eliminar
                        </button>
                      </td>
                    </tr>
                  </tbody>
                </table>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

// Array de items del inventario.
// Cada item será un objeto: { id, nombre, cantidad, valor, adquirido }
const items = ref([])

// Campos del formulario
const nuevoNombre = ref('')
const nuevaCantidad = ref(1)
const nuevoValor = ref(0)

// Mensaje de error del formulario
const errorFormulario = ref('')

// Contador para generar ids únicos
let siguienteId = 1

/**
 * Agrega un nuevo item al inventario.
 * Valida que el nombre no esté vacío y la cantidad sea >= 1.
 */
function agregarItem() {
  // Limpiamos errores previos
  errorFormulario.value = ''

  // Validaciones básicas
  if (nuevoNombre.value.trim() === '') {
    errorFormulario.value = 'El nombre del item no puede estar vacío.'
    return
  }

  if (nuevaCantidad.value < 1) {
    errorFormulario.value = 'La cantidad debe ser al menos 1.'
    return
  }

  // Creamos el nuevo item
  const item = {
    id: siguienteId++,
    nombre: nuevoNombre.value.trim(),
    cantidad: nuevaCantidad.value,
    valor: nuevoValor.value,
    adquirido: false
  }

  // Lo agregamos al array
  items.value.push(item)

  // Limpiamos el formulario
  nuevoNombre.value = ''
  nuevaCantidad.value = 1
  nuevoValor.value = 0
}

/**
 * Elimina un item del inventario por su id.
 */
function eliminarItem(id) {
  items.value = items.value.filter(item => item.id !== id)
}
</script>

<style scoped>
.inventory-card {
  background: rgba(17, 24, 39, 0.8);
  border: 1px solid #374151;
  border-radius: 12px;
  box-shadow: 0 0 20px rgba(99, 102, 241, 0.2);
}

.inventory-title {
  color: #818cf8;
  font-weight: 800;
  letter-spacing: 1px;
  text-transform: uppercase;
  font-size: 1.6rem;
}

.inventory-subtitle {
  color: #a5b4fc;
  font-size: 0.9rem;
  margin-bottom: 0;
}

.inventory-input {
  background: rgba(31, 41, 55, 0.7);
  border: 1px solid #374151;
  color: #e5e7eb;
}

.inventory-input:focus {
  background: rgba(31, 41, 55, 0.9);
  border-color: #818cf8;
  color: #e5e7eb;
  box-shadow: 0 0 0 0.2rem rgba(129, 140, 248, 0.25);
}

.inventory-btn {
  background: linear-gradient(90deg, #6366f1, #818cf8);
  border: none;
  font-weight: 600;
  color: #0b0f19;
}

.inventory-btn:hover {
  background: linear-gradient(90deg, #4f46e5, #6366f1);
}

.inventory-alert-warning {
  background: rgba(251, 191, 36, 0.1);
  color: #fde68a;
  border: 1px solid rgba(251, 191, 36, 0.4);
}

.inventory-empty {
  padding: 2rem 0;
}

.inventory-status {
  color: #a5b4fc;
  font-size: 0.95rem;
}

.inventory-table {
  background: rgba(17, 24, 39, 0.6);
  color: #e5e7eb;
}

.inventory-table th {
  color: #a5b4fc;
  border-color: #374151;
}

.inventory-table td {
  border-color: #374151;
}

.inventory-row {
  transition: background 0.2s;
}

.inventory-row:hover {
  background: rgba(99, 102, 241, 0.1);
}

.item-adquirido {
  opacity: 0.6;
  text-decoration: line-through;
}

.inventory-btn-delete {
  background: rgba(239, 68, 68, 0.2);
  border: 1px solid rgba(239, 68, 68, 0.5);
  color: #fca5a5;
}

.inventory-btn-delete:hover {
  background: rgba(239, 68, 68, 0.4);
}
</style>