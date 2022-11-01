<template>
    <v-container id="formularioAxios" fluid tag="section">
        <v-row>
            <v-col cols="4">
                <v-card elevation="2">
                    <v-card-title>
                        Formulario con axios
                    </v-card-title>
                    <v-card-subtitle>
                        Introduce tus datos
                    </v-card-subtitle>
                    <v-spacer></v-spacer>
                    <v-card-actions>
                        <v-row>
                            <v-col cols="12" md="11">
                                <validation-observer v-slot="{ handleSubmit }">
                                    <form @submit="handleSubmit(handleClick)">
                                        <v-row align="center" dense>
                                            <v-col cols="12">
                                                <validation-provider v-slot="{errors}"
                                                name="nombre"
                                                rules="required|min:3|max:20">
                                                <v-text-field v-model="nombre"
                                                :error-messages="errors"
                                                label="Nombre" color="secondary">
                                                </v-text-field>
                                                </validation-provider>

                                                <validation-provider v-slot="{errors}"
                                                name="apellidos"
                                                rules="required|min:5|max:50">
                                                <v-text-field v-model="apellidos"
                                                :error-messages="errors"
                                                label="Apellidos" color="secondary">
                                                </v-text-field>
                                                </validation-provider>

                                                <validation-provider v-slot="{errors}"
                                                name="telefono"
                                                rules="required|min:10|max:10|numeric">
                                                <v-text-field v-model="telefono"
                                                :error-messages="errors"
                                                label="Telefono" color="secondary">
                                                </v-text-field>
                                                </validation-provider>

                                                <validation-provider v-slot="{errors}"
                                                name="email"
                                                rules="required|min:8|max:40|email">
                                                <v-text-field v-model="email"
                                                :error-messages="errors"
                                                label="E-mail" color="secondary">
                                                </v-text-field>
                                                </validation-provider>

                                                <v-spacer></v-spacer>

                                                <div class="pa-3 text-center">
                                                <v-btn class="ma-2"
                                                outlined
                                                color="purple" type="submit">
                                                <v-icon dark right>mdi-checkbox-marked-circle</v-icon>
                                                Agregar
                                                </v-btn>
                                                </div>
                                            </v-col>
                                        </v-row></form>
                                </validation-observer>
                            </v-col>
                        </v-row>
                        

                    </v-card-actions>
                </v-card>
            </v-col>
        <!--</v-row>
        <v-row>-->
            <v-col cols="6">
                <v-card elevation="2">
                    <v-card-title>
                        Datos BD
                    </v-card-title>
                    <v-spacer></v-spacer>
                    <v-card-actions>
                        <div class="col-md-12">
                        <table class="table table-striped">
                <thead>
                    <tr>
                        <!--<th>ID</th>-->
                        <th>Nombre</th>
                        <th>Apellidos</th>
                        <th>Email</th>
                        <th>Telefono</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="registros in info" :key="registros.key">
                        <!--<td>{{ user.key }}</td>-->
                        <td>{{ registros.nombre }}</td>
                        <td>{{ registros.apellidos }}</td>
                        <td>{{ registros.email }}</td>
                        <td>{{ registros.telefono }}</td>
                    </tr>
                </tbody>
            </table></div>
                    </v-card-actions>
                </v-card>
            </v-col>
        </v-row>
    </v-container>
</template>
<script>
import axios from 'axios'
import { required, max, min, numeric,email} from 'vee-validate/dist/rules'
import { extend, setInteractionMode } from 'vee-validate'

setInteractionMode('eager')
extend('max',{
    ...max,
    message:'El campo {_field_} no debe tener más de {length} caracteres',
})
extend('min',{
    ...min,
    message:'El campo {_field_} debe tener al menos {length} caracteres',
})
extend('required',{
    ...required,
    message:'El campo {_field_} no debe estar vacío',
})
extend('email',{
    ...email,
    message:'El campo {_field_} debe ser un email',
})
extend('numeric',{
    ...numeric,
    message:'El campo {_field_} debe ser un numero',
})
export default{
    name: 'RolesView',
    $_veeValidate: {
        validator:'new',
    },
    data: () => ({
        info:[],
        valores:'',
        nombre:'',
        apellidos:'',
        telefono:'',
        email:'',
        rol: {
            nombre:'',
        },
    }),
    mounted () {
    axios
        .get("https://formulariovue-f6243-default-rtdb.firebaseio.com/registros.json")
        .then(response => (this.info = response.data))
    },
    methods: {
        handleClick: function() {
            axios.post('https://formulariovue-f6243-default-rtdb.firebaseio.com/registros.json', {
            nombre: this.nombre,
            apellidos: this.apellidos,
            telefono: this.telefono,
            email: this.email
        })
        .then(response => {
            console.log(response)
        })
        .catch(err => console.error(err));
    },
        validarFormRoles(){
            alert('se validaron los campos')
        }
    },
}
</script>
<style>

</style>