<template>
  <div class="modal fade show" tabindex="-1" role="dialog" style="display: block;">
    <div class="modal-dialog" role="document">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title">{{ internalUser.id ? 'Editar Usuário' : 'Adicionar Usuário' }}</h5>
          <button type="button" class="btn-close" aria-label="Close" @click="close" aria-hidden="true"></button>
        </div>
        <div class="modal-body">
          <form @submit.prevent="updateUser">
            <div class="mb-3">
              <label for="name" class="form-label">Nome</label>
              <input type="text" class="form-control" id="name" v-model="internalUser.name" required>
            </div>
            <div class="mb-3">
              <label for="age" class="form-label">Idade</label>
              <input type="number" class="form-control" id="age" v-model="internalUser.age" required>
            </div>
            <div class="mb-3">
              <label for="cpf" class="form-label">CPF</label>
              <input type="text" class="form-control" id="cpf" v-model="internalUser.cpf" required>
            </div>
            <div class="mb-3">
              <label for="email" class="form-label">Email</label>
              <input type="email" class="form-control" id="email" v-model="internalUser.email" required>
            </div>
            <button type="submit" class="btn btn-primary">Salvar</button>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  props: {
    user: {
      type: Object,
      default: () => ({ name: '', age: '', cpf: '', email: '' }),
    },
  },
  data() {
    return {
      internalUser: { ...this.user },
    };
  },
  methods: {
    async updateUser() {
      try {
        if (this.internalUser.id) {
          await axios.put(`http://localhost:3000/pessoas/${this.internalUser.id}`, this.internalUser);
        } else {
          await axios.post('http://localhost:3000/pessoas', this.internalUser);
        }
        this.$emit('save');
        this.close();
      } catch (error) {
        console.error('Erro ao atualizar usuário:', error);
        alert('Erro ao atualizar usuário. Por favor, tente novamente mais tarde.');
      }
    },
    close() {
      this.$emit('close');
    },
  },
  watch: {
    user(newUser) {
      this.internalUser = { ...newUser };
    },
  },
};
</script>

<style scoped>
.modal {
  display: block;
  background: rgba(0, 0, 0, 0.5);
}
</style>