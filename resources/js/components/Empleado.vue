<template>
    <div>
        <h1 class="text-center">Gestionar Empleados</h1>
        <hr>

        <!-- Button to Open the Modal -->
        <button @click="modificar=false; openModal();" type="button" class="btn btn-primary">
            Nuevo
        </button>

        <!-- The Modal -->
        <div class="modal" :class="{mostrar:modal}">
        <div class="modal-dialog">
            <div class="modal-content">

            <!-- Modal Header -->
            <div class="modal-header">
                <h4 class="modal-title">{{ tituloModal}}</h4>
                <button @click="closeModal();" type="button" class="close" data-dismiss="modal">&times;</button>
            </div>

            <!-- Modal body -->
            <div class="modal-body">
               
               <form action="">

                   

               </form>



            </div>

            <!-- Modal footer -->
            <div class="modal-footer">
                <button @click="closeModal();" type="button" class="btn btn-secondary" data-dismiss="modal">Cancelar</button>
                <button type="button" class="btn btn-success" data-dismiss="modal">Guardar</button>
        
            </div>

            </div>
        </div>
        </div>

        <table class="table table-striped">
        <thead class="thead-dark">
            <tr>
            <th scope="col">#</th>
            <th scope="col">Name</th>
            <th scope="col">Last Name</th>
            <th scope="col">Phone</th>
            <th scope="col">DNI</th>
            <th scope="col">Direction</th>

            <th scope="col" colspan="2" class="text-center">Acciones</th>

            </tr>
        </thead>
        <tbody>
            <tr v-for="empl in empleados" :key="empl.id">
            <th scope="row">{{ empl.id }}</th>
                <td>{{ empl.name }}</td>
                <td>{{ empl.last_name }}</td>
                <td>{{ empl.phone }}</td>
                <td>{{ empl.dni }}</td>
                <td>{{ empl.direction }}</td>
                <td class="text-center">
                    <button @click="modificar=true; openModal(empl.id);" class="btn btn-warning">Editar</button>
                    <button @click="elinimar(empl.id)" class="btn btn-danger">Eliminar</button>
                </td>               

            </tr>
        </tbody>
        </table>

    </div>
</template>

<script>
export default {

    data(){
        return {
            modificar:true,
            modal:0,
            tituloModal:'',
            empleados:[],
        }
    },

    methods: {
        async listar() {
            const res = await axios.get('/empleados');
            this.empleados = res.data;
        },

        async elinimar(id) {
            const res = await axios.delete('/empleados/'+id);
            this.listar();

        },

        openModal (id=0) {
            this.modal = 1;
            if(this.modificar){
                this.tituloModal="Modificar Empleado";
            } else {
                this.tituloModal="Crear Empleado"
            }
        },

        closeModal () {
            this.modal = 0;
        },
    },

    created() {
        this.listar();
    } 

}
</script>

<style>
    .mostrar {
        display: list-item;
        opacity: 1;
        background: rgb(51, 36, 36);
    }
</style>