<template>
  <div class="container">
    <h1>{{ title }}</h1>
    <p>
      <button type="button" class="btn btn-primary" @click="goToNewContact()">
        <i class="bi bi-person-plus"></i>
      </button>
    </p>
    <div class="mb-3">
      <div class="input-group">
        <span class="input-group-text" id="basic-addon3">
          Filter by country
          <i class="bi bi-filter"></i>
        </span>
        <select v-model="filterCountryBy" id="color" class="form-select">
          <option disabled value="">-- Selecciona --</option>
          <option value="">All</option>
          <option v-for="marca in countries" :key="marca" :value="marca">
            {{ marca }}
          </option>
        </select>
      </div>
    </div>
    <div class="mb-3">
      <div class="input-group">
        <span class="input-group-text" id="basic-addon3">
          Search by name
          <i class="bi bi-search"></i>
        </span>
        <input type="search" v-model="modeloAbuscar" class="form-control">
      </div>
    </div>
    <div class="table-responsive">
      <table class="table table-bordered border-primary">
        <thead>
        <tr>
          <th scope="col">id</th>
          <th scope="col">
            name
            <i class="bi bi-person"></i>
          </th>
          <th scope="col">
            email
            <i class="bi bi-envelope"></i>
          </th>
          <th scope="col">
            address
            <i class="bi bi-map"></i>
          </th>
          <th scope="col">
            phone
            <i class="bi bi-telephone"></i>
          </th>
          <th scope="col">
            country
            <i class="bi bi-globe-americas"></i>
          </th>
          <th scope="col">
            city
            <i class="bi bi-building"></i>
          </th>
          <th></th>
        </tr>
        </thead>
        <tbody>
        <tr v-for="(item, index) in getItems" :key="item.id">
          <th scope="row">{{ item.id }}</th>
          <td>{{ item.name }}</td>
          <td>{{ item.email }}</td>
          <td>{{ item.address }}</td>
          <td>{{ item.phone }}</td>
          <td>{{ item.country }}</td>
          <td>{{ item.city }}</td>
          <td>
            <button type="button" class="btn btn-primary" @click="openModal(index)" title="edit">
              <i class="bi bi-pencil"></i>
            </button>
            <button type="button" class="btn btn-danger" @click="deleteContact(index)" title="delete">
              <i class="bi bi-trash"></i>
            </button>
          </td>
        </tr>
        </tbody>
      </table>
    </div>

  </div>


  <!-- Modal Bootstrap -->
  <div class="modal fade" id="modalContact" tabindex="-1" aria-labelledby="modalContactLabel" aria-hidden="true"
       ref="modalRef">
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header">
          <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Cerrar"></button>
        </div>
        <div class="modal-body">
          <!-- Componente AutoEditar -->
          <EditContact v-if="modalMode === 'editar' && contactSelected" :contact="contactSelected"
                       @update="storeEdition"
                       @cancelar="closeModal"/>
          <NewContact v-if="modalMode === 'createContact'" @created="addContact($event)"></NewContact>
        </div>
      </div>
    </div>
  </div>

</template>

<script>
import 'bootstrap-icons/font/bootstrap-icons.css';
import NewContact from '../components/contacts/NewContact.component.vue';
import EditContact from '../components/contacts/EditContact.component.vue';


export default {
  name: 'AgendaView',
  data() {
    return {
      title: 'Contacts',
      items: [
        {
          id: 1,
          name: "Alice Johnson",
          email: "alice.johnson@example.com",
          address: "123 Maple Street",
          phone: "123-456-7890",
          country: "USA",
          city: "New York"
        },
        {
          id: 2,
          name: "Bob Smith",
          email: "bob.smith@example.com",
          address: "456 Oak Avenue",
          phone: "987-654-3210",
          country: "Canada",
          city: "Toronto"
        },
        {
          id: 3,
          name: "Carol White",
          email: "carol.white@example.com",

          address: "789 Pine Road",
          phone: "555-123-4567",
          country: "UK",
          city: "London"
        },
        {
          id: 4,
          name: "David Brown",
          email: "david.brown@example.com",
          address: "321 Elm Street",
          phone: "444-555-6666",
          country: "Australia",
          city: "Sydney"
        },
        {
          id: 5,
          name: "Emily Davis",
          email: "emily.davis@example.com",
          address: "654 Spruce Lane",
          phone: "333-444-5555",
          country: "USA",
          city: "Los Angeles"
        }
      ],
      modalBootstrapInstance: null,
      contactSelected: null,
      indiceSeleccionado: 0,
      countries: [
        'USA',
        'Canada',
        'UK',
        'Australia',
      ],
      filterCountryBy: "",
      modeloAbuscar: "",
      modalMode: "createContact"
    }
  },
  components: {
    NewContact,
    EditContact,
  },
  mounted() {
    this.$nextTick(() => {
      if (this.$refs.modalRef) {
        // eslint-disable-next-line
        this.modalBootstrapInstance = new bootstrap.Modal(this.$refs.modalRef);
      } else {
        console.error('No se encontró el ref modalRef');
      }
    });
  },
  methods: {
    // métodos que se pueden llamar desde la plantilla o desde otras partes del componente.
    goToNewContact() {
      this.modalMode = "createContact";
      if (this.modalBootstrapInstance) {
        this.modalBootstrapInstance.show();

      } else {
        console.error('modalBootstrapInstance no está inicializado');
      }
    },
    openModal(index) {
      this.contactSelected = null;
      this.indiceSeleccionado = index;
      this.modalMode = "editar";
      setTimeout(() => {
        if (this.modalBootstrapInstance) {
          this.modalBootstrapInstance.show();
          this.contactSelected = {...this.items[index]};
        } else {
          console.error('modalBootstrapInstance is not initiated');
        }
      });

    },
    closeModal() {
      if (this.modalBootstrapInstance) {
        this.modalBootstrapInstance.hide();
      }
    },
    storeEdition(editedContact) {
      console.log('Auto editado:', editedContact);
      // TODO: add info related to API to persist
      this.items[this.indiceSeleccionado] = editedContact;
      this.closeModal();
    },
    deleteContact(index) {
      if (confirm('¿Are you sure remove this contact?')) {
        this.items.splice(index, 1);
      }
    },
    addContact(newContact) {
      const maxId = Math.max(...this.items.map(auto => auto.id));
      newContact['id'] = maxId + 1;
      this.items.push(newContact);
      //TODO: use localstorage to persist new contacts
      console.log(newContact);
      this.closeModal();
    }

  },
  computed: {
    // propiedades computadas que dependen de otras propiedades reactivas
    getItems() {
      let result = [...this.items];
      if (this.filterCountryBy !== "") {
        result = this.items
            .filter((item) => item.country === this.filterCountryBy);
      }

      if (this.anioAbuscar !== "") {
        result = result.filter((item) => {
          const _marca = item.modelo || "";
          const _textToSearch = this.modeloAbuscar || "";
          return _marca.toLowerCase().includes(_textToSearch.toLocaleLowerCase());
        });
      }

      return result;
    }
  },
  props: {
    // propiedades que el componente puede recibir.
  },
  emits: [] // los eventos personalizados que el componente puede emitir.
}
</script>

<style></style>