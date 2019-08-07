<template>
  <div class="animated fadeIn">
    <b-card header="Personas" header-tag="header" footer-tag="footer">
      <router-link to="/alta-persona">
        <b-button variant="blue"><v-icon name="plus"/> Nuevo</b-button>
      </router-link>
      <br />
      <br />
      <div v-if="items != null">
        <b-table :small="true" responsive striped hover :items="items" :fields="fields">
          <template slot="actions" slot-scope="row">
            <b-button size="sm" @click="editPersona(row.item, row.index)" class="mr-2" variant="blue"><v-icon name="edit"/></b-button>
            <b-button size="sm" @click="deletePersona(row.item, row.index)" class="mr-2"><v-icon name="trash-alt"/></b-button>
          </template>
        </b-table>
      </div>
      <span v-else>Cargando Personas...</span>
    </b-card>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "personas",
  mounted() {
    this.getPersonas();
  },
  data() {
    return {
      items: null,
      fields: {
        perId: {
          label: "Id",
          sortable: true
        },
        perNombre: {
          label: "Nombre",
          sortable: true
        },
        perApellido: {
          label: "Apellido",
          sortable: true
        },
        perNumeroDocumento: {
          label: "Nro Documento",
          sortable: false
        },
        perTipoDocumento: {
          label: "Tipo Documento",
          sortable: false
        },
        perFechaNacimiento: {
          label: "Fecha Nacimiento",
          sortable: false
        },
        actions: {
          label: "",
          sortable: false
        }
      }
    };
  },
  methods: {
    getPersonas() {
      axios.get("http://localhost:8080/personas").then(respuesta => {
        this.items = respuesta.data.data;
        this.items.forEach(element => {
          element.perTipoDocumento = element.perTipoDocumento.tpoDescripcion;
        });
      });
    }
  }
};
</script>
