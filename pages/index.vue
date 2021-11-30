<template>
  <b-container fluid class="p-5">
    <h1>CRUD con SQLServer</h1>

    <b-row class="my-2">
      <b-col>
        <b-table :items="clientes" responsive striped hover small></b-table>
      </b-col>
      <b-col cols="12" class="d-flex justify-content-end">
        <b-button @click="obtenerClientes">Recargar clientes</b-button>
      </b-col>
    </b-row>

    <b-form @submit="onSubmit">
      <b-form-group label="Nombre" label-for="nombre">
        <b-form-input id="nombre" v-model="nombre"></b-form-input>
      </b-form-group>
      <b-form-group label="Apellido Paterno" label-for="apellido_paterno">
        <b-form-input
          id="apellido_paterno"
          v-model="apellido_paterno"
        ></b-form-input>
      </b-form-group>
      <b-form-group label="Apellido Materno" label-for="apellido_materno">
        <b-form-input
          id="apellido_materno"
          v-model="apellido_materno"
        ></b-form-input>
      </b-form-group>
      <b-form-group label="Num. exterior" label-for="num_exterior">
        <b-form-input id="num_exterior" v-model="num_exterior"></b-form-input>
      </b-form-group>
      <b-form-group label="RFC" label-for="rfc">
        <b-form-input id="rfc" v-model="rfc"></b-form-input>
      </b-form-group>
      <b-form-group label="Colonia" label-for="colonia">
        <b-form-input id="colonia" v-model="colonia"></b-form-input>
      </b-form-group>
      <b-form-group label="Calle" label-for="calle">
        <b-form-input id="calle" v-model="calle"></b-form-input>
      </b-form-group>
      <b-form-group label="Estado" label-for="estado">
        <b-form-input id="estado" v-model="estado"></b-form-input>
      </b-form-group>
      <b-form-group label="Municipio" label-for="municipio">
        <b-form-input id="municipio" v-model="municipio"></b-form-input>
      </b-form-group>
      <b-form-group label="Fecha de nacimiento" label-for="fecha_nacimiento">
        <b-form-input
          id="fecha_nacimiento"
          v-model="fecha_nacimiento"
        ></b-form-input>
      </b-form-group>
      <b-form-group label="Género" label-for="genero">
        <b-form-input id="genero" v-model="genero"></b-form-input>
      </b-form-group>

      <b-button type="submit" variant="primary" :disabled="isDisabled">
        Guardar
      </b-button>
    </b-form>
  </b-container>
</template>

<script>
export default {
  data() {
    return {
      clientes: [],
      nombre: '',
      apellido_paterno: '',
      apellido_materno: '',
      num_exterior: '',
      rfc: '',
      colonia: '',
      calle: '',
      estado: '',
      municipio: '',
      fecha_nacimiento: '',
      genero: '',
      error_message: '',
    }
  },

  computed: {
    isDisabled() {
      return (
        !this.nombre ||
        !this.apellido_paterno ||
        !this.apellido_materno ||
        !this.num_exterior ||
        !this.rfc ||
        !this.colonia ||
        !this.calle ||
        !this.estado ||
        !this.municipio ||
        !this.fecha_nacimiento ||
        !this.genero
      )
    },
  },

  async mounted() {
    await this.obtenerClientes()
  },

  methods: {
    async crearCliente() {
      const data = {
        numExt: this.num_exterior,
        rfc: this.rfc,
        colonia: this.colonia,
        municipio: this.municipio,
        estado: this.estado,
        genero: this.genero,
        calle: this.calle,
        nombre: this.nombre,
        apellidoP: this.apellido_paterno,
        apellidoM: this.apellido_materno,
        fechaNacimiento: this.fecha_nacimiento,
      }

      try {
        const cliente = await this.$axios.$post('/crear_cliente', data)
        this.clientes.push(cliente)
      } catch (error) {
        this.error_message = error.message
      }
    },
    async obtenerClientes() {
      const clientes = await this.$axios.$get('/obtener_clientes')
      this.clientes = clientes.map((cliente) => {
        return {
          id: cliente.idCliente,
          nombre: cliente.nombre,
          apellidoP: cliente.apellidoP,
          apellidoM: cliente.apellidoM,
          numExt: cliente.numExt,
          rfc: cliente.rfc,
          colonia: cliente.colonia,
          municipio: cliente.municipio,
          estado: cliente.estado,
          calle: cliente.calle,
          fechaNacimiento: cliente.fechaNacimiento,
          genero: cliente.genero,
        }
      })
    },

    onSubmit(evt) {
      evt.preventDefault()
      this.crearCliente()
    },
  },
}
</script>
