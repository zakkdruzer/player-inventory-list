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
            <!-- Aquí irá el formulario y la lista -->
            <p class="text-center inventory-status">
              Inventario vacío. Agrega tu primer item.
            </p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
// Por ahora no hay lógica, solo estructura.
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

.inventory-status {
  color: #a5b4fc;
  font-size: 0.95rem;
}
</style>