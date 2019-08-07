<template>
  <div class="animated fadeIn">
    <b-card header="Datos Generales" header-tag="header" footer-tag="footer">
      <div>
        <b-form>
          <div>
            <b-card bg-variant="light">
              <b-form-group>
                <b-row>
                  <b-col class="pad-x">
                    <b-form-group>
                      <b-row>TipoDocumento:</b-row>
                      <b-row>
                        <b-form-select
                          id="comboTipoDocumento"
                          v-model="tipoDocumentoSelected"
                          @change="getSelectedItem"
                          :options="tipoDocumentoOptions"
                        >
                          <template slot="first">
                            <option :value="null" disabled>-- Seleccionar una opcion --</option>
                          </template>
                        </b-form-select>
                      </b-row>
                    </b-form-group>
                  </b-col>
                  <b-col class="pad-x">
                    <b-form-group>
                      <b-row>Folio:</b-row>
                      <b-row>
                        <b-form-input
                          type="number"
                          id="folio"
                          min="10000"
                          v-model="personaDTO.folio"
                        ></b-form-input>
                      </b-row>
                    </b-form-group>
                  </b-col>
                  <b-col class="pad-x">
                    <b-form-group>
                      <b-row>Descripción:</b-row>
                      <b-row>
                        <b-form-input id="descripcion" v-model="personaDTO.descripcion"></b-form-input>
                      </b-row>
                    </b-form-group>
                  </b-col>
                  <b-col class="pad-x">
                    <b-form-group>
                      <b-row>Observación:</b-row>
                      <b-row>
                        <b-form-input id="observacion" v-model="personaDTO.observacion"></b-form-input>
                      </b-row>
                    </b-form-group>
                  </b-col>
                </b-row>
              </b-form-group>
            </b-card>
          </div>
        </b-form>
      </div>
    </b-card>
  </div>
</template>

<script>
import axios from 'axios';
export default {
	name: 'alta-persona',
	mounted() {
		this.getTiposDocumento();
	},
	data() {
		return {
			tipoDocumentoSelected: null,
			tipoDocumentoOptions: {},
			personaDto: {
				perApellido: null,
				perFechaNacimiento: null,
				perId: null,
				perNombre: null,
				perNumeroDocumento: null,
				perTpoId: null
			}
		};
	},
	methods: {
		getTiposDocumento() {
			axios.get('http://localhost:8080/documentos').then(respuesta => {
				this.tipoDocumentoOptions = respuesta.data.data;
			});
		},
		getSelectedItem(tipoDocumentoSelected) {
			this.personaDto.perTpoId = tipoDocumentoSelected;
		},
		savePersona() {
			const mensaje = this.validar();
			if (mensaje === null) {
				axios
					.post('http://localhost:8686/personas', this.personaDto)
					.then(respuesta => {
						this.makeToast(
							'success',
							'Felicitaciones!',
							'Se ha guardado correctamente',
							true
						);
					})
					.catch(error => {
						console.log(error);
						this.makeToast('danger', 'Ups!', 'Ha ocurrido un error', false);
					});
			} else {
				this.makeToast('warning', 'Ups!', mensaje, false);
			}
		},
		validar() {
			let mensaje = null;
			if (this.tipoDocumentoSelected === null) {
				mensaje = 'Debe seleccionar un tipoDocumento';
			}
			return mensaje;
		},
		makeToast(variant = null, titulo, msj, redirect) {
			const router = this.$router;
			setTimeout(resolve => {
				this.$bvToast.toast(`${msj}`, {
					title: `${titulo}`,
					variant: variant,
					solid: true
				});
			}, 1000);
			if (redirect) router.push('/personas');
		}
	}
};
</script>

<style>
</style>