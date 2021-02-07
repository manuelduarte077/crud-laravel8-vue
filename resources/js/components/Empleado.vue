<template>
    <div>
        <h1 class="text-center">Gestionar Empleados</h1>
        <hr />

        <!-- Button to Open the Modal -->
        <button
            @click="
                modificar = false;
                openModal();
            "
            type="button"
            class="btn btn-primary mb-3"
        >
            Nuevo
        </button>

        <!-- The Modal -->
        <div class="modal" :class="{ mostrar: modal }">
            <div class="modal-dialog">
                <div class="modal-content">
                    <!-- Modal Header -->
                    <div class="modal-header">
                        <h4 class="modal-title">{{ tituloModal }}</h4>
                        <button
                            @click="closeModal()"
                            type="button"
                            class="close"
                            data-dismiss="modal"
                        >
                            &times;
                        </button>
                    </div>

                    <!-- Modal body -->
                    <div class="modal-body">
                        <form action="">

                            <div class="mb-3">
                                <label for="name" class="form-label">Name</label>
                                <input v-model="empleado.name" type="text" class="form-control" id="name" placeholder="Nombre"/>
                            </div>

                            <div class="mb-3">
                                <label for="last_name" class="form-label">LastName</label>
                                <input v-model="empleado.last_name" type="text" class="form-control" id="last_name" placeholder="Apellido"/>
                            </div>

                             <div class="mb-3">
                                <label for="phone" class="form-label">Phone</label>
                                <input v-model="empleado.phone" type="number" class="form-control" id="phone" placeholder="Celular"/>
                            </div>
                            <div class="mb-3">
                                <label for="dni" class="form-label">DNI</label>
                                <input v-model="empleado.dni" type="number" class="form-control" id="dni" placeholder="Cedula"/>
                            </div>

                            <div class="mb-3">
                                <label for="direction" class="form-label">Dirección</label>
                               <textarea v-model="empleado.direction" class="form-control" id="direction" rows="3"></textarea>
                            </div>

                        </form>
                    </div>

                    <!-- Modal footer -->
                    <div class="modal-footer">
                        <button @click="closeModal()" type="button" class="btn btn-secondary" data-dismiss="modal">
                            Cancelar
                        </button>
                        <button @click="guardar()" type="button" class="btn btn-success" data-dismiss="modal" >
                            Guardar
                        </button>
                    </div>
                </div>
            </div>
        </div>

        <table class="table table-striped table-bordered" id="empleado" style="width:100%">
            <thead class="bg-primary text-white">
                <tr>
                    <th scope="col">#</th>
                    <th scope="col">Name</th>
                    <th scope="col">Last Name</th>
                    <th scope="col">Phone</th>
                    <th scope="col">DNI</th>
                    <th scope="col">Direction</th>

                    <th scope="col" colspan="2" class="text-center">
                        Acciones
                    </th>
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
                        <button
                            @click="
                                modificar = true;
                                openModal(empl);
                            "
                            class="btn btn-warning"
                        >
                            Editar
                        </button>
                        <button
                            @click="elinimar(empl.id)"
                            class="btn btn-danger"
                        >
                            Eliminar
                        </button>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>



export default {
    data() {
        return {
            empleado:{
                name:'',
                last_name:'',
                phone: 1,
                dni: 1,
                direction:''

            },

            modificar: true,
            modal: 0,
            tituloModal: "",
            empleados: []
        };
    },

    methods: {
        async listar() {
            const res = await axios.get("/empleados");
            this.empleados = res.data;
        },

        async elinimar(id) {
            const res = await axios.delete("/empleados/" + id);
            this.listar();
        },


        async guardar(id) {

            if(this.modificar){
                
                const res = await axios.put('/empleados/'+ this.id, this.empleado);
                
            } else {
                
                const res = await axios.post('/empleados', this.empleado);

            }
            this.closeModal();
            this.listar();

        },

        openModal(data={}) {
            this.modal = 1;
            if (this.modificar) {
                this.tituloModal = "Modificar Empleado";
                this.id=data.id,

                this.empleado.name = data.name;
                this.empleado.last_name = data.last_name;
                this.empleado.phone = data.phone;
                this.empleado.dni = data.dni;
                this.empleado.direction = data.direction;

            } else {
                this.id=0,
                this.tituloModal = "Crear Empleado";

                this.empleado.name = '';
                this.empleado.last_name = '';
                this.empleado.phone = '';
                this.empleado.dni = '';
                this.empleado.direction = '';
            }
        },

        closeModal() {
            this.modal = 0;
        },   
        
        mytable() {
        $(document).ready(function() {
            $('#empleado').DataTable({
                "lengthMenu": [[5,10, -1], [5,10,50, "ALL"]]
            });
        } );
        }

    },

    created() {
        this.listar();
        this.mytable();
    }

    
};

</script>

<style>
.mostrar {
    display: list-item;
    opacity: 1;
    background: rgb(51, 36, 36);
}
</style>
