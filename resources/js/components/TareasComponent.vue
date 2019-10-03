<template>
    <div>
        <h3>Agregar tareas</h3>
   
    <form @submit.prevent = "agregar">
        <input type="text" placeholder="Nombre" class="form-control mb-2" v-model="nota.nombre">
        <input type="text"   placeholder="Descripcion" class="form-control mb-2" v-model="nota.descripcion">
        <button type="submit" class="btn btn-primary">Agregar</button>
    </form>

    <div class="mt-3">
    <h3>Listado de nota</h3>
    <ul class="list-group" my-2>
        <li class="list-group-item"
        v-for="(item, index) in notas" :key="index">
        <span class="badge badge-primary float-right">
            {{item.updated_at}}
        </span>
        <p>{{item.nombre}}</p>
        <p>{{item.descripcion}}</p>
        <button class="btn btn-danger btn-sm" @click="eliminarNota(item, index)">Eliminar</button>
        </li>
    </ul>
    </div>
     </div>
</template>

<script>
export default {
    data(){
        return {
            notas: [],
            nota:{nombre:'',descripcion:''}
        }
    },
    created(){
        axios.get('/notas')
        .then(res=>{
            this.notas = res.data;
        })
    },
    methods:{
        agregar(){
            if(this.nota.nombre.trim() === '' || this.nota.descripcion.trim() === ''){
                alert('debes rellenar los campos');
                return 0;
            }
            const params = {nombre: this.nota.nombre, descripcion: this.nota.descripcion};
            this.nota.nombre = '';
            this.nota.descripcion = '';
            axios.post('/notas', params)
            .then( res =>{
                this.notas.push(res.data);
            });
        },
        eliminarNota(item, index){
            axios.delete(`/notas/${item.id}`)
            .then(()=>{
                this.notas.splice(index, 1);
            })
        }
    }
}
</script>
